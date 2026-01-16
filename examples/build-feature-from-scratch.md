# 🚀 Building Features from Scratch with Master Prompt

## 🎯 Example: React Task Management Dashboard

### TASK CONTEXT:

Build a comprehensive Task Management Dashboard for a SaaS productivity application.

**Requirements:**
- Real-time task list with filtering and sorting
- Task creation modal with rich text description
- Drag-and-drop task prioritization
- Progress tracking with visual indicators
- Team collaboration features (assignments, comments)
- Responsive design for desktop and mobile

**Technical Stack:**
- React 18 + TypeScript
- Tailwind CSS for styling
- React Query for data management
- React Hook Form for forms
- Framer Motion for animations
- Date-fns for date handling

**Constraints:**
- Must be fully accessible (WCAG 2.1 compliant)
- Should support offline functionality
- Must be optimized for performance (LCP < 2.5s)
- Should follow modern React patterns (hooks, context, custom hooks)
- Must be easily testable

---

## 🧠 THINK → Deep Problem Understanding

### 1. Task Understanding
This is a core productivity feature that needs to handle:
- Real-time data synchronization across users
- Complex state management for tasks and UI
- Accessibility requirements for diverse users
- Performance optimization for large task lists
- Offline capability for mobile usage

**Success looks like:**
- Users can efficiently manage tasks with minimal friction
- Application feels responsive even with 1000+ tasks
- Accessible to screen readers and keyboard navigation
- Works seamlessly offline with sync on reconnect

### 2. Assumptions & Constraints
**Technical:**
- Backend API exists with REST endpoints for tasks
- WebSocket support for real-time updates
- Authentication system in place
- File upload service available for attachments

**Business:**
- Target users: teams of 2-50 people
- Expected concurrent users: 100-500
- Data retention: 2 years minimum

**Platform:**
- Modern browsers (Chrome, Safari, Firefox, Edge)
- Mobile Safari and Chrome Mobile
- Screen reader compatibility required

### 3. Strategy & Approach
**Why this approach:**
- React Query for optimized data fetching and caching
- Custom hooks for business logic separation
- Component composition for reusability
- Optimistic UI for better user experience

**Alternatives considered:**
- Redux: Overkill for this scope, React Query + Context sufficient
- CSS-in-JS: Tailwind provides better performance and smaller bundle
- Class components: Hooks provide better composition and testing

**Trade-offs:**
- Bundle size vs feature richness
- Real-time updates vs performance
- Offline complexity vs user experience

---

## 📋 PLAN → Structured Execution Steps

### Phase 1: Foundation (Day 1)
1. **Project Structure Setup**
   - Component architecture planning
   - TypeScript configuration
   - Testing setup (Jest + React Testing Library)
   - Storybook for component documentation

2. **Core Infrastructure**
   - API service layer with React Query integration
   - Authentication context provider
   - Error boundary setup
   - Loading states skeleton

### Phase 2: Core Features (Days 2-3)
1. **Task List Component**
   - Virtualized scrolling for performance
   - Filtering and sorting logic
   - Responsive grid layout
   - Accessibility attributes

2. **Task Management**
   - Create/Edit/Delete operations
   - Optimistic updates implementation
   - Undo/Redo capability
   - Local storage for offline support

### Phase 3: Advanced Features (Days 4-5)
1. **Drag & Drop**
   - Priority reordering
   - Status column changes
   - Touch device support

2. **Collaboration Features**
   - Real-time comments
   - User mentions
   - Notification system
   - Activity timeline

### Phase 4: Polish & Optimization (Day 6)
1. **Performance Optimization**
   - Bundle splitting
   - Image optimization
   - Memory leak prevention
   - Lighthouse score optimization

2. **Accessibility**
   - Screen reader testing
   - Keyboard navigation
   - Color contrast compliance
   - Focus management

---

## 🔍 RESEARCH → Modern Best Practices (2026)

### Data Fetching Strategy
```typescript
// Using React Query with optimistic updates
const { data: tasks, isLoading, error } = useQuery({
  queryKey: ['tasks'],
  queryFn: fetchTasks,
  staleTime: 5 * 60 * 1000, // 5 minutes
});

const { mutate: updateTask } = useMutation({
  mutationFn: updateTaskApi,
  onMutate: async (newTask) => {
    // Cancel any outgoing refetches
    await queryClient.cancelQueries({ queryKey: ['tasks'] });
    
    // Snapshot the previous value
    const previousTasks = queryClient.getQueryData(['tasks']);
    
    // Optimistically update to the new value
    queryClient.setQueryData(['tasks'], (old: Task[]) => 
      old.map(task => task.id === newTask.id ? newTask : task)
    );
    
    return { previousTasks };
  },
  onError: (err, newTask, context) => {
    // Rollback on error
    queryClient.setQueryData(['tasks'], context.previousTasks);
  },
  onSettled: () => {
    // Always refetch after error or success
    queryClient.invalidateQueries({ queryKey: ['tasks'] });
  },
});
```

### Performance Optimization
- **Virtualization**: React Window for large lists
- **Memoization**: React.memo + useMemo + useCallback
- **Code Splitting**: React.lazy for route-based splitting
- **Bundle Analysis**: Webpack Bundle Analyzer integration

### Accessibility Patterns
- ARIA labels for interactive elements
- Keyboard navigation with focus traps
- Screen reader announcements for state changes
- High contrast mode support

---

## 🛠️ EXECUTE → Production-Grade Output

### Folder Structure
```
src/
├── components/
│   ├── tasks/
│   │   ├── TaskList/
│   │   │   ├── TaskList.tsx
│   │   │   ├── TaskList.test.tsx
│   │   │   ├── TaskList.stories.tsx
│   │   │   └── index.ts
│   │   ├── TaskCard/
│   │   ├── TaskForm/
│   │   └── TaskFilters/
│   └── ui/
│       ├── Modal/
│       ├── Button/
│       └── Loader/
├── hooks/
│   ├── useTasks.ts
│   ├── useTaskMutations.ts
│   └── useOfflineSync.ts
├── services/
│   ├── api.ts
│   ├── websocket.ts
│   └── storage.ts
├── types/
│   └── task.ts
├── utils/
│   ├── date.ts
│   ├── validation.ts
│   └── accessibility.ts
└── contexts/
    ├── AuthContext.tsx
    └── TaskContext.tsx
```

### Core Component: TaskList
```tsx
// src/components/tasks/TaskList/TaskList.tsx
import React from 'react';
import { FixedSizeList as List } from 'react-window';
import { useTasks } from '../../../hooks/useTasks';
import { TaskCard } from '../TaskCard/TaskCard';
import { TaskSkeleton } from '../TaskSkeleton/TaskSkeleton';
import { EmptyState } from '../../ui/EmptyState/EmptyState';

interface TaskListProps {
  filters?: TaskFilters;
  onTaskClick?: (task: Task) => void;
}

export const TaskList: React.FC<TaskListProps> = ({ 
  filters, 
  onTaskClick 
}) => {
  const { 
    tasks, 
    isLoading, 
    error, 
    hasNextPage, 
    fetchNextPage 
  } = useTasks(filters);

  const Row = ({ index, style }: { index: number; style: React.CSSProperties }) => {
    const task = tasks[index];
    
    if (!task) {
      return <div style={style}><TaskSkeleton /></div>;
    }

    return (
      <div style={style}>
        <TaskCard 
          task={task} 
          onClick={() => onTaskClick?.(task)}
          aria-label={`Task: ${task.title}`}
        />
      </div>
    );
  };

  if (isLoading && tasks.length === 0) {
    return (
      <div className="space-y-3">
        {Array.from({ length: 5 }).map((_, i) => (
          <TaskSkeleton key={i} />
        ))}
      </div>
    );
  }

  if (error) {
    return (
      <EmptyState 
        title="Unable to load tasks"
        description="Please try again later"
        icon="error"
      />
    );
  }

  if (tasks.length === 0) {
    return (
      <EmptyState 
        title="No tasks found"
        description="Create your first task to get started"
        icon="empty"
      />
    );
  }

  return (
    <div 
      role="list" 
      aria-label="Task list"
      className="space-y-2"
    >
      <List
        height={600}
        itemCount={tasks.length}
        itemSize={100}
        width="100%"
        onItemsRendered={({ visibleStopIndex }) => {
          if (visibleStopIndex >= tasks.length - 5 && hasNextPage) {
            fetchNextPage();
          }
        }}
      >
        {Row}
      </List>
    </div>
  );
};
```

### Custom Hook: useTasks
```typescript
// src/hooks/useTasks.ts
import { useQuery, useInfiniteQuery } from '@tanstack/react-query';
import { Task, TaskFilters } from '../types/task';
import { api } from '../services/api';
import { useDebounce } from './useDebounce';

export const useTasks = (filters?: TaskFilters) => {
  const debouncedFilters = useDebounce(filters, 300);

  return useInfiniteQuery({
    queryKey: ['tasks', debouncedFilters],
    queryFn: async ({ pageParam = 1 }) => {
      const response = await api.get<Task[]>('/tasks', {
        params: { 
          ...debouncedFilters, 
          page: pageParam, 
          limit: 20 
        },
      });
      return response.data;
    },
    getNextPageParam: (lastPage, allPages) => {
      return lastPage.length === 20 ? allPages.length + 1 : undefined;
    },
    staleTime: 5 * 60 * 1000,
    keepPreviousData: true,
  });
};

// Optimistic mutation hook
export const useTaskMutations = () => {
  const queryClient = useQueryClient();

  const createTask = useMutation({
    mutationFn: (newTask: Partial<Task>) => api.post('/tasks', newTask),
    onMutate: async (newTask) => {
      await queryClient.cancelQueries({ queryKey: ['tasks'] });
      
      const previousTasks = queryClient.getQueryData(['tasks']);
      
      queryClient.setQueryData(['tasks'], (old: Task[] = []) => [
        { ...newTask, id: Date.now().toString(), _optimistic: true },
        ...old,
      ]);
      
      return { previousTasks };
    },
    onError: (err, newTask, context) => {
      queryClient.setQueryData(['tasks'], context?.previousTasks);
    },
    onSettled: () => {
      queryClient.invalidateQueries({ queryKey: ['tasks'] });
    },
  });

  return { createTask };
};
```

### Offline Support Hook
```typescript
// src/hooks/useOfflineSync.ts
import { useEffect } from 'react';
import { useQueryClient } from '@tanstack/react-query';
import { storage } from '../services/storage';

const OFFLINE_QUEUE_KEY = 'offline_task_queue';

export const useOfflineSync = () => {
  const queryClient = useQueryClient();

  useEffect(() => {
    const handleOnline = async () => {
      const queue = await storage.get<OfflineTask[]>(OFFLINE_QUEUE_KEY) || [];
      
      if (queue.length > 0) {
        // Process offline queue
        for (const task of queue) {
          try {
            await api.post('/tasks', task);
            await storage.remove(OFFLINE_QUEUE_KEY, task.id);
          } catch (error) {
            console.error('Failed to sync task:', task.id, error);
          }
        }
        
        // Invalidate queries to refresh data
        queryClient.invalidateQueries({ queryKey: ['tasks'] });
      }
    };

    window.addEventListener('online', handleOnline);
    
    return () => {
      window.removeEventListener('online', handleOnline);
    };
  }, [queryClient]);

  const addToOfflineQueue = async (task: Partial<Task>) => {
    const queue = await storage.get<OfflineTask[]>(OFFLINE_QUEUE_KEY) || [];
    queue.push({ ...task, _offline: true, timestamp: Date.now() });
    await storage.set(OFFLINE_QUEUE_KEY, queue);
  };

  return { addToOfflineQueue };
};
```

---

## ✅ Quality & Review Checklist

### Performance
- [ ] Lighthouse score > 90
- [ ] LCP < 2.5 seconds
- [ ] CLS < 0.1
- [ ] Bundle size < 200KB gzipped
- [ ] Memory usage stable with 1000+ items

### Accessibility (WCAG 2.1)
- [ ] Screen reader compatibility tested
- [ ] Keyboard navigation complete
- [ ] Color contrast ratio > 4.5:1
- [ ] Focus indicators visible
- [ ] ARIA attributes implemented
- [ ] Error messages accessible

### Testing
- [ ] Unit tests > 80% coverage
- [ ] Integration tests for user flows
- [ ] E2E tests for critical paths
- [ ] Performance testing with large datasets
- [ ] Cross-browser testing completed

### Security
- [ ] XSS protection implemented
- [ ] CSRF tokens handled
- [ ] Input validation on client and server
- [ ] Secure HTTP headers
- [ ] Content Security Policy

### Developer Experience
- [ ] TypeScript strict mode enabled
- [ ] ESLint and Prettier configured
- [ ] Storybook documentation complete
- [ ] Component API documented
- [ ] Error boundaries implemented

---

## 🚀 Future Enhancements

### Immediate (Post-Launch)
- **Real-time collaboration**: Live cursor presence, simultaneous editing
- **Advanced filtering**: Saved filters, complex query builder
- **Keyboard shortcuts**: Vim-like navigation, quick actions
- **Export functionality**: CSV, PDF, Excel exports

### Medium Term (Q2 2026)
- **AI features**: Smart task suggestions, auto-categorization
- **Integration ecosystem**: Slack, Google Calendar, Jira sync
- **Mobile app**: React Native version
- **Advanced analytics**: Time tracking, productivity insights

### Long Term (H2 2026)
- **Machine learning**: Predictive task scheduling
- **Voice interface**: Voice commands and dictation
- **AR/VR support**: Spatial task management
- **Blockchain integration**: Decentralized task verification

### Without Refactor
- **Plugin system**: Extensible via npm packages
- **Theming engine**: Dynamic theme switching
- **Internationalization**: Multi-language support
- **Accessibility features**: Can be layered incrementally

---

## 🎯 Expected Output Level

This implementation is production-ready and:
- ✅ **Developers** can implement directly with clear patterns
- ✅ **Product Managers** can approve based on comprehensive specs
- ✅ **Designers** can verify UX patterns and accessibility
- ✅ **QA Team** has clear testing criteria
- ✅ **Stakeholders** can understand business value and roadmap

The architecture follows modern React best practices, is performant, accessible, and maintainable by teams of any size.