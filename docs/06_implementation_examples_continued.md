# Brotea: Implementation Examples (Continued)

This document continues the implementation examples for the Brotea platform, focusing on frontend implementation, database schema, and testing strategies.

## Frontend Implementation Examples

### 1. React Component for Learning Module

```tsx
// src/components/learning/LearningModuleCard.tsx
import React from 'react';
import { Card, CardContent, CardActions, Typography, Button, Chip, Box, LinearProgress } from '@mui/material';
import { styled } from '@mui/material/styles';
import { useNavigate } from 'react-router-dom';
import { LearningModule } from '../../types/learning';

const StyledCard = styled(Card)(({ theme }) => ({
  height: '100%',
  display: 'flex',
  flexDirection: 'column',
  transition: 'transform 0.2s ease-in-out',
  '&:hover': {
    transform: 'translateY(-4px)',
    boxShadow: theme.shadows[4],
  },
}));

const DifficultyChip = styled(Chip)<{ difficulty: string }>(({ theme, difficulty }) => {
  const colors = {
    beginner: theme.palette.success.main,
    intermediate: theme.palette.warning.main,
    advanced: theme.palette.error.main,
  };
  
  return {
    backgroundColor: colors[difficulty as keyof typeof colors] || theme.palette.primary.main,
    color: theme.palette.common.white,
    fontWeight: 'bold',
  };
});

interface LearningModuleCardProps {
  module: LearningModule;
  progress?: number;
}

export const LearningModuleCard: React.FC<LearningModuleCardProps> = ({ module, progress = 0 }) => {
  const navigate = useNavigate();
  
  const handleModuleClick = () => {
    navigate(`/learning/modules/${module.id}`);
  };
  
  return (
    <StyledCard>
      <CardContent sx={{ flexGrow: 1 }}>
        <Box display="flex" justifyContent="space-between" alignItems="center" mb={2}>
          <Typography variant="h5" component="h2" gutterBottom>
            {module.title}
          </Typography>
          <DifficultyChip
            label={module.difficulty.charAt(0).toUpperCase() + module.difficulty.slice(1)}
            difficulty={module.difficulty}
            size="small"
          />
        </Box>
        
        <Typography variant="body2" color="text.secondary" paragraph>
          {module.description}
        </Typography>
        
        <Box mt={2}>
          <Typography variant="body2" color="text.secondary">
            {module.topics?.length || 0} topics
          </Typography>
          
          {progress > 0 && (
            <Box mt={1}>
              <Typography variant="body2" color="text.secondary">
                Progress: {Math.round(progress)}%
              </Typography>
              <LinearProgress 
                variant="determinate" 
                value={progress} 
                sx={{ mt: 0.5, height: 8, borderRadius: 4 }} 
              />
            </Box>
          )}
        </Box>
      </CardContent>
      
      <CardActions>
        <Button size="small" color="primary" onClick={handleModuleClick}>
          {progress > 0 ? 'Continue Learning' : 'Start Learning'}
        </Button>
      </CardActions>
    </StyledCard>
  );
};
```

### 2. React Component for AI Assistant Interaction

```tsx
// src/components/ai/AIAssistantChat.tsx
import React, { useState, useRef, useEffect } from 'react';
import { 
  Box, 
  TextField, 
  Button, 
  Paper, 
  Typography, 
  Avatar, 
  CircularProgress,
  Divider,
  IconButton,
} from '@mui/material';
import { styled } from '@mui/material/styles';
import SendIcon from '@mui/icons-material/Send';
import CodeIcon from '@mui/icons-material/Code';
import SmartToyIcon from '@mui/icons-material/SmartToy';
import PersonIcon from '@mui/icons-material/Person';
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';
import { atomDark } from 'react-syntax-highlighter/dist/esm/styles/prism';
import { useAIAssistant } from '../../hooks/useAIAssistant';
import { Message, MessageType } from '../../types/ai';
import ReactMarkdown from 'react-markdown';

const ChatContainer = styled(Paper)(({ theme }) => ({
  display: 'flex',
  flexDirection: 'column',
  height: '600px',
  padding: theme.spacing(2),
  backgroundColor: theme.palette.background.default,
}));

const MessagesContainer = styled(Box)(({ theme }) => ({
  flexGrow: 1,
  overflow: 'auto',
  padding: theme.spacing(2),
  display: 'flex',
  flexDirection: 'column',
  gap: theme.spacing(2),
}));

const MessageBubble = styled(Box)<{ isUser: boolean }>(({ theme, isUser }) => ({
  display: 'flex',
  alignItems: 'flex-start',
  maxWidth: '80%',
  alignSelf: isUser ? 'flex-end' : 'flex-start',
}));

const MessageContent = styled(Paper)<{ isUser: boolean }>(({ theme, isUser }) => ({
  padding: theme.spacing(2),
  borderRadius: theme.spacing(2),
  backgroundColor: isUser ? theme.palette.primary.main : theme.palette.background.paper,
  color: isUser ? theme.palette.primary.contrastText : theme.palette.text.primary,
  marginLeft: isUser ? 0 : theme.spacing(1),
  marginRight: isUser ? theme.spacing(1) : 0,
}));

const InputContainer = styled(Box)(({ theme }) => ({
  display: 'flex',
  padding: theme.spacing(2),
  gap: theme.spacing(1),
}));

interface AIAssistantChatProps {
  assistantType: 'education' | 'development';
  initialContext?: string;
}

export const AIAssistantChat: React.FC<AIAssistantChatProps> = ({ 
  assistantType, 
  initialContext 
}) => {
  const [input, setInput] = useState('');
  const messagesEndRef = useRef<HTMLDivElement>(null);
  const { messages, isLoading, sendMessage } = useAIAssistant(assistantType, initialContext);

  const handleSendMessage = () => {
    if (input.trim() && !isLoading) {
      sendMessage(input);
      setInput('');
    }
  };

  const handleKeyPress = (e: React.KeyboardEvent) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      handleSendMessage();
    }
  };

  // Auto-scroll to bottom when messages change
  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  // Render code blocks with syntax highlighting
  const renderMessageContent = (message: Message) => {
    if (message.type === MessageType.USER) {
      return <Typography>{message.content}</Typography>;
    }
    
    return (
      <ReactMarkdown
        components={{
          code({ node, inline, className, children, ...props }) {
            const match = /language-(\w+)/.exec(className || '');
            return !inline && match ? (
              <SyntaxHighlighter
                style={atomDark}
                language={match[1]}
                PreTag="div"
                {...props}
              >
                {String(children).replace(/\n$/, '')}
              </SyntaxHighlighter>
            ) : (
              <code className={className} {...props}>
                {children}
              </code>
            );
          }
        }}
      >
        {message.content}
      </ReactMarkdown>
    );
  };

  return (
    <ChatContainer elevation={3}>
      <Typography variant="h6" gutterBottom>
        {assistantType === 'education' ? 'Web3 Education Assistant' : 'Mobile Development Assistant'}
      </Typography>
      <Divider />
      
      <MessagesContainer>
        {messages.map((message, index) => (
          <MessageBubble key={index} isUser={message.type === MessageType.USER}>
            <Avatar>
              {message.type === MessageType.USER ? (
                <PersonIcon />
              ) : (
                <SmartToyIcon />
              )}
            </Avatar>
            <MessageContent isUser={message.type === MessageType.USER}>
              {renderMessageContent(message)}
            </MessageContent>
          </MessageBubble>
        ))}
        
        {isLoading && (
          <MessageBubble isUser={false}>
            <Avatar>
              <SmartToyIcon />
            </Avatar>
            <MessageContent isUser={false}>
              <Box display="flex" alignItems="center">
                <CircularProgress size={20} />
                <Typography ml={1}>Thinking...</Typography>
              </Box>
            </MessageContent>
          </MessageBubble>
        )}
        
        <div ref={messagesEndRef} />
      </MessagesContainer>
      
      <InputContainer>
        <TextField
          fullWidth
          variant="outlined"
          placeholder={`Ask the ${assistantType} assistant...`}
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyPress={handleKeyPress}
          multiline
          maxRows={4}
          disabled={isLoading}
        />
        <IconButton 
          color="primary" 
          onClick={handleSendMessage} 
          disabled={isLoading || !input.trim()}
        >
          <SendIcon />
        </IconButton>
        <IconButton color="secondary">
          <CodeIcon />
        </IconButton>
      </InputContainer>
    </ChatContainer>
  );
};
```

### 3. React Component for Component Repository

```tsx
// src/components/repository/ComponentCard.tsx
import React from 'react';
import { 
  Card, 
  CardContent, 
  CardActions, 
  Typography, 
  Button, 
  Chip, 
  Box, 
  Rating,
  Stack,
  Divider,
} from '@mui/material';
import { styled } from '@mui/material/styles';
import CodeIcon from '@mui/icons-material/Code';
import ForkRightIcon from '@mui/icons-material/ForkRight';
import DownloadIcon from '@mui/icons-material/Download';
import { Component } from '../../types/component';
import { useNavigate } from 'react-router-dom';

const StyledCard = styled(Card)(({ theme }) => ({
  height: '100%',
  display: 'flex',
  flexDirection: 'column',
  transition: 'all 0.2s ease-in-out',
  '&:hover': {
    transform: 'translateY(-4px)',
    boxShadow: theme.shadows[4],
  },
}));

const PlatformChip = styled(Chip)<{ platform: string }>(({ theme, platform }) => {
  const colors = {
    ios: theme.palette.info.main,
    android: theme.palette.success.main,
    'cross-platform': theme.palette.warning.main,
    web: theme.palette.secondary.main,
  };
  
  return {
    backgroundColor: colors[platform as keyof typeof colors] || theme.palette.primary.main,
    color: theme.palette.common.white,
    fontWeight: 'bold',
  };
});

interface ComponentCardProps {
  component: Component;
  onFork?: (componentId: string) => void;
  onDownload?: (componentId: string) => void;
}

export const ComponentCard: React.FC<ComponentCardProps> = ({ 
  component, 
  onFork, 
  onDownload 
}) => {
  const navigate = useNavigate();
  
  const handleViewDetails = () => {
    navigate(`/repository/components/${component.id}`);
  };
  
  const handleFork = (e: React.MouseEvent) => {
    e.stopPropagation();
    if (onFork) {
      onFork(component.id);
    }
  };
  
  const handleDownload = (e: React.MouseEvent) => {
    e.stopPropagation();
    if (onDownload) {
      onDownload(component.id);
    }
  };
  
  return (
    <StyledCard>
      <CardContent sx={{ flexGrow: 1 }}>
        <Box display="flex" justifyContent="space-between" alignItems="center" mb={2}>
          <Typography variant="h6" component="h2">
            {component.name}
          </Typography>
          <PlatformChip
            label={component.platform.replace('-', ' ')}
            platform={component.platform}
            size="small"
            icon={<CodeIcon fontSize="small" />}
          />
        </Box>
        
        <Typography variant="body2" color="text.secondary" paragraph>
          {component.description}
        </Typography>
        
        <Stack direction="row" spacing={1} flexWrap="wrap" mb={2}>
          {component.tags.map((tag) => (
            <Chip key={tag} label={tag} size="small" variant="outlined" />
          ))}
        </Stack>
        
        <Divider sx={{ my: 1 }} />
        
        <Box display="flex" justifyContent="space-between" alignItems="center">
          <Box display="flex" alignItems="center">
            <Rating 
              value={component.usageMetrics.avgRating} 
              precision={0.5} 
              size="small" 
              readOnly 
            />
            <Typography variant="body2" color="text.secondary" ml={0.5}>
              ({component.usageMetrics.reviewsCount})
            </Typography>
          </Box>
          
          <Box display="flex" alignItems="center" gap={1}>
            <Box display="flex" alignItems="center">
              <ForkRightIcon fontSize="small" color="action" />
              <Typography variant="body2" color="text.secondary" ml={0.5}>
                {component.usageMetrics.forks}
              </Typography>
            </Box>
            
            <Box display="flex" alignItems="center">
              <DownloadIcon fontSize="small" color="action" />
              <Typography variant="body2" color="text.secondary" ml={0.5}>
                {component.usageMetrics.downloads}
              </Typography>
            </Box>
          </Box>
        </Box>
      </CardContent>
      
      <CardActions>
        <Button size="small" color="primary" onClick={handleViewDetails}>
          View Details
        </Button>
        <Button size="small" startIcon={<ForkRightIcon />} onClick={handleFork}>
          Fork
        </Button>
        <Button size="small" startIcon={<DownloadIcon />} onClick={handleDownload}>
          Download
        </Button>
      </CardActions>
    </StyledCard>
  );
};
```

### 4. Redux Toolkit Query API Slice

```typescript
// src/store/api/learningApi.ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';
import { RootState } from '../store';
import { 
  LearningModule, 
  Topic, 
  Content, 
  Exercise, 
  Progress,
  ModuleFilters,
} from '../../types/learning';

export const learningApi = createApi({
  reducerPath: 'learningApi',
  baseQuery: fetchBaseQuery({ 
    baseUrl: '/api',
    prepareHeaders: (headers, { getState }) => {
      // Get token from auth state
      const token = (getState() as RootState).auth.token;
      
      // Add authorization header if token exists
      if (token) {
        headers.set('authorization', `Bearer ${token}`);
      }
      
      return headers;
    },
  }),
  tagTypes: ['Module', 'Topic', 'Progress'],
  endpoints: (builder) => ({
    // Learning Modules
    getModules: builder.query<LearningModule[], ModuleFilters | void>({
      query: (filters) => ({
        url: 'learning-modules',
        params: filters,
      }),
      providesTags: (result) =>
        result
          ? [
              ...result.map(({ id }) => ({ type: 'Module' as const, id })),
              { type: 'Module', id: 'LIST' },
            ]
          : [{ type: 'Module', id: 'LIST' }],
    }),
    
    getModuleById: builder.query<LearningModule, string>({
      query: (id) => `learning-modules/${id}`,
      providesTags: (result, error, id) => [{ type: 'Module', id }],
    }),
    
    createModule: builder.mutation<LearningModule, Partial<LearningModule>>({
      query: (module) => ({
        url: 'learning-modules',
        method: 'POST',
        body: module,
      }),
      invalidatesTags: [{ type: 'Module', id: 'LIST' }],
    }),
    
    updateModule: builder.mutation<LearningModule, { id: string; module: Partial<LearningModule> }>({
      query: ({ id, module }) => ({
        url: `learning-modules/${id}`,
        method: 'PUT',
        body: module,
      }),
      invalidatesTags: (result, error, { id }) => [
        { type: 'Module', id },
        { type: 'Module', id: 'LIST' },
      ],
    }),
    
    deleteModule: builder.mutation<void, string>({
      query: (id) => ({
        url: `learning-modules/${id}`,
        method: 'DELETE',
      }),
      invalidatesTags: [{ type: 'Module', id: 'LIST' }],
    }),
    
    // Topics
    getTopicsByModuleId: builder.query<Topic[], string>({
      query: (moduleId) => `learning-modules/${moduleId}/topics`,
      providesTags: (result) =>
        result
          ? [
              ...result.map(({ id }) => ({ type: 'Topic' as const, id })),
              { type: 'Topic', id: 'LIST' },
            ]
          : [{ type: 'Topic', id: 'LIST' }],
    }),
    
    getTopicById: builder.query<Topic, string>({
      query: (id) => `topics/${id}`,
      providesTags: (result, error, id) => [{ type: 'Topic', id }],
    }),
    
    // Progress
    getUserModuleProgress: builder.query<Progress, string>({
      query: (moduleId) => `learning-modules/${moduleId}/progress`,
      providesTags: (result, error, moduleId) => [
        { type: 'Progress', id: moduleId },
      ],
    }),
    
    updateProgress: builder.mutation<Progress, { entityId: string; entityType: string; status: string; score?: number }>({
      query: (progressData) => ({
        url: 'progress',
        method: 'POST',
        body: progressData,
      }),
      invalidatesTags: (result, error, { entityId, entityType }) => [
        { type: 'Progress', id: entityId },
      ],
    }),
  }),
});

export const {
  useGetModulesQuery,
  useGetModuleByIdQuery,
  useCreateModuleMutation,
  useUpdateModuleMutation,
  useDeleteModuleMutation,
  useGetTopicsByModuleIdQuery,
  useGetTopicByIdQuery,
  useGetUserModuleProgressQuery,
  useUpdateProgressMutation,
} = learningApi;
```

## Database Schema Implementation

### 1. PostgreSQL Migration for User Tables

```sql
-- migrations/001_create_user_tables.sql

-- Users Table
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) NOT NULL UNIQUE,
  display_name VARCHAR(255) NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  role VARCHAR(20) NOT NULL DEFAULT 'user',
  is_active BOOLEAN NOT NULL DEFAULT TRUE,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Create index on email for faster lookups
CREATE INDEX idx_users_email ON users(email);

-- Profiles Table
CREATE TABLE profiles (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  bio TEXT,
  experience_level VARCHAR(20) DEFAULT 'beginner',
  preferences JSONB DEFAULT '{}',
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Create unique index on user_id to ensure one profile per user
CREATE UNIQUE INDEX idx_profiles_user_id ON profiles(user_id);

-- User Skills Table
CREATE TABLE user_skills (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  skill_name VARCHAR(100) NOT NULL,
  proficiency_level INTEGER NOT NULL DEFAULT 1,
  added_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Create index on user_id for faster lookups
CREATE INDEX idx_user_skills_user_id ON user_skills(user_id);
-- Create unique index on user_id and skill_name to prevent duplicates
CREATE UNIQUE INDEX idx_user_skills_unique ON user_skills(user_id, skill_name);

-- Function to update the updated_at timestamp
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
   NEW.updated_at = NOW();
   RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Trigger for users table
CREATE TRIGGER update_users_updated_at
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();

-- Trigger for profiles table
CREATE TRIGGER update_profiles_updated_at
BEFORE UPDATE ON profiles
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();
```

### 2. PostgreSQL Migration for Learning Module Tables

```sql
-- migrations/002_create_learning_module_tables.sql

-- Learning Modules Table
CREATE TABLE learning_modules (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title VARCHAR(255) NOT NULL,
  description TEXT NOT NULL,
  difficulty_level VARCHAR(20) NOT NULL DEFAULT 'beginner',
  metadata JSONB DEFAULT '{}',
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Topics Table
CREATE TABLE topics (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  module_id UUID NOT NULL REFERENCES learning_modules(id) ON DELETE CASCADE,
  title VARCHAR(255) NOT NULL,
  sequence_order INTEGER NOT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Create index on module_id for faster lookups
CREATE INDEX idx_topics_module_id ON topics(module_id);
-- Create index on sequence_order for ordered retrieval
CREATE INDEX idx_topics_sequence ON topics(module_id, sequence_order);

-- Content Table
CREATE TABLE content (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  topic_id UUID NOT NULL REFERENCES topics(id) ON DELETE CASCADE,
  content_type VARCHAR(50) NOT NULL,
  data JSONB NOT NULL,
  metadata JSONB DEFAULT '{}',
  sequence_order INTEGER NOT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Create index on topic_id for faster lookups
CREATE INDEX idx_content_topic_id ON content(topic_id);
-- Create index on sequence_order for ordered retrieval
CREATE INDEX idx_content_sequence ON content(topic_id, sequence_order);

-- Exercises Table
CREATE TABLE exercises (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  topic_id UUID NOT NULL REFERENCES topics(id) ON DELETE CASCADE,
  instructions TEXT NOT NULL,
  solution_template TEXT,
  validation_criteria JSONB NOT NULL,
  difficulty_level VARCHAR(20) NOT NULL DEFAULT 'beginner',
  sequence_order INTEGER NOT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Create index on topic_id for faster lookups
CREATE INDEX idx_exercises_topic_id ON exercises(topic_id);
-- Create index on sequence_order for ordered retrieval
CREATE INDEX idx_exercises_sequence ON exercises(topic_id, sequence_order);

-- Module Prerequisites Table
CREATE TABLE module_prerequisites (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  module_id UUID NOT NULL REFERENCES learning_modules(id) ON DELETE CASCADE,
  prerequisite_id UUID NOT NULL REFERENCES learning_modules(id) ON DELETE CASCADE,
  CONSTRAINT unique_prerequisite UNIQUE (module_id, prerequisite_id)
);

-- Create index on module_id for faster lookups
CREATE INDEX idx_module_prerequisites_module_id ON module_prerequisites(module_id);

-- Progress Table
CREATE TABLE user_progress (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  entity_id UUID NOT NULL,
  entity_type VARCHAR(50) NOT NULL,
  completion_status VARCHAR(20) NOT NULL DEFAULT 'not_started',
  score INTEGER,
  last_accessed TIMESTAMP NOT NULL DEFAULT NOW(),
  notes TEXT,
  CONSTRAINT unique_user_entity UNIQUE (user_id, entity_id, entity_type)
);

-- Create index on user_id for faster lookups
CREATE INDEX idx_user_progress_user_id ON user_progress(user_id);
-- Create index on entity_id for faster lookups
CREATE INDEX idx_user_progress_entity_id ON user_progress(entity_id);
-- Create composite index for common queries
CREATE INDEX idx_user_progress_composite ON user_progress(user_id, entity_type, completion_status);

-- Triggers for updated_at columns
CREATE TRIGGER update_learning_modules_updated_at
BEFORE UPDATE ON learning_modules
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_topics_updated_at
BEFORE UPDATE ON topics
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_content_updated_at
BEFORE UPDATE ON content
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_exercises_updated_at
BEFORE UPDATE ON exercises
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();
```

## Testing Strategy Implementation

### 1. Unit Test for User Service

```typescript
// src/application/services/__tests__/user.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { JwtService } from '@nestjs/jwt';
import { EventEmitter2 } from '@nestjs/event-emitter';
import { ConflictException, NotFoundException, UnauthorizedException } from '@nestjs/common';
import * as bcrypt from 'bcrypt';
import { UserService } from '../user.service';
import { UserRepository } from '../../../infrastructure/repositories/user.repository';
import { CreateUserDto } from '../../dtos/user/create-user.dto';
import { LoginDto } from '../../dtos/auth/login.dto';
import { User, UserRole } from '../../../domain/entities/user.entity';

// Mock bcrypt
jest.mock('bcrypt');

describe('UserService', () => {
  let service: UserService;
  let userRepository: UserRepository;
  let jwtService: JwtService;
  let eventEmitter: EventEmitter2;

  const mockUser: User = {
    id: '123',
    email: 'test@example.com',
    displayName: 'Test User',
    passwordHash: 'hashedpassword',
    role: UserRole.USER,
    isActive: true,
    createdAt: new Date(),
    updatedAt: new Date(),
    profileInfo: {
      bio: '',
      skills: [],
      experience: 'beginner',
      preferences: {},
    },
    createdComponents: [],
    learningProgress: [],
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UserService,
        {
          provide: UserRepository,
          useValue: {
            findAll: jest.fn(),
            findById: jest.fn(),
            findByEmail: jest.fn(),
            create: jest.fn(),
            update: jest.fn(),
            delete: jest.fn(),
            getUserWithProgress: jest.fn(),
          },
        },
        {
          provide: JwtService,
          useValue: {
            sign: jest.fn(),
          },
        },
        {
          provide: EventEmitter2,
          useValue: {
            emit: jest.fn(),
          },
        },
      ],
    }).compile();

    service = module.get<UserService>(UserService);
    userRepository = module.get<UserRepository>(UserRepository);
    jwtService = module.get<JwtService>(JwtService);
    eventEmitter = module.get<EventEmitter2>(EventEmitter2);
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });

  describe('findById', () => {
    it('should return a user if found', async () => {
      jest.spyOn(userRepository, 'findById').mockResolvedValue(mockUser);
      
      const result = await service.findById('123');
      
      expect(result).toEqual(mockUser);
      expect(userRepository.findById).toHaveBeenCalledWith('123');
    });

    it('should throw NotFoundException if user not found', async () => {
      jest.spyOn(userRepository, 'findById').mockResolvedValue(null);
      
      await expect(service.findById('123')).rejects.toThrow(NotFoundException);
    });
  });

  describe('register', () => {
    const createUserDto: CreateUserDto = {
      email: 'test@example.com',
      password: 'password123',
      displayName: 'Test User',
    };

    it('should create a new user successfully', async () => {
      jest.spyOn(userRepository, 'findByEmail').mockResolvedValue(null);
      jest.spyOn(bcrypt, 'hash').mockResolvedValue('hashedpassword' as never);
      jest.spyOn(userRepository, 'create').mockResolvedValue(mockUser);
      jest.spyOn(eventEmitter, 'emit');
      
      const result = await service.register(createUserDto);
      
      expect(result).toEqual(mockUser);
      expect(userRepository.findByEmail).toHaveBeenCalledWith(createUserDto.email);
      expect(bcrypt.hash).toHaveBeenCalledWith(createUserDto.password, 10);
      expect(userRepository.create).toHaveBeenCalledWith(expect.objectContaining({
        email: createUserDto.email,
        displayName: createUserDto.displayName,
        passwordHash: 'hashedpassword',
      }));
      expect(eventEmitter.emit).toHaveBeenCalledWith(
        'user.registered',
        expect.any(Object),
      );
    });

    it('should throw ConflictException if user already exists',
