# GC25 Project Documentation

## Overview

GC25 is a comprehensive system with multiple backend services communicating through gRPC. The project is designed for scheduling meetings, generating reports, handling chat functionality, and managing updates through an asynchronous architecture.

## System Architecture

The system consists of the following core components:

1. **gc-auth-backend**: Authentication service written in Python
2. **gc-chat-backend**: Chat functionality service written in Python
3. **gc-report-backend**: Report generation service written in Python
4. **gc-scheduler-backend**: Meeting scheduling service with asynchronous task processing
5. **gc-update-backend**: Update management service with asynchronous task processing

## Backend Services

### gc-auth-backend

Handles user authentication and authorization for the entire system.

**Key Components:**
- User authentication API
- Token generation and validation
- Role-based access control

### gc-chat-backend

A Python-based service that provides chat functionality, with gRPC integration to communicate with other services, particularly the scheduler service.

**Key Components:**
- gRPC client integration with scheduler service
- REST API endpoints for chat functionality
- WebSocket support for real-time communication

### gc-report-backend

Handles report generation and processing functionality.

**Key Components:**
- Report generation engine
- Data processing and visualization
- PDF/document export capabilities

### gc-scheduler-backend

A task scheduling and email notification system built with gRPC, Celery, Redis, and MongoDB.

**Key Features:**
- gRPC API with `ScheduleMeet` endpoint
- Asynchronous task execution with Celery
- Email notifications with HTML templating
- MongoDB integration for data persistence
- Redis for message brokering and tracking
- Automatic retry mechanism for failed tasks
- Scheduled tasks with Celery Beat

**Architecture:**
1. **gRPC Server**: Exposes endpoints and validates requests
2. **Celery Workers**: Process tasks asynchronously
3. **Redis**: Message broker for Celery
4. **MongoDB**: Data persistence for tasks and user details
5. **Email Template System**: Customizable HTML templates
6. **Celery Beat**: Manages periodic task scheduling

### gc-update-backend

Handles system updates and synchronization across services.

**Key Components:**
- Celery workers for asynchronous update processing
- Celery Beat for scheduled updates
- Integration with other services for consistency

## Running the Project

### Prerequisites

Before running the project, ensure you have:

1. Python 3.x installed
2. Redis server running (for Celery)
3. MongoDB running (for data persistence)
4. A terminal emulator (xterm, konsole, terminator, or gnome-terminal)

### Steps to Run the Shell Script

1. **Navigate to the project directory**:
   ```bash
   cd /path/to/gc25
   ```

2. **Make the script executable** (if not already):
   ```bash
   chmod +x start_backends.sh
   ```

3. **Run the script**:
   ```bash
   ./start_backends.sh
   ```

4. **What happens when you run the script**:
   - The script will set up virtual environments for each backend service
   - It will install required dependencies from requirements.txt files
   - It will launch each service in a separate terminal window
   - For scheduler and update backends, it will start both Celery worker and beat processes

5. **Monitoring**:
   - After running the script, you'll have multiple terminal windows open
   - Each window shows logs for a specific service
   - Keep these terminals open to monitor the services

### Troubleshooting

- If no terminal windows open, the script will display manual commands to run
- If you encounter permission issues, ensure you have execute permissions for the script
- If services fail to start, check that Redis and MongoDB are running
- For dependency issues, check the terminal output for specific error messages

## Communication Between Services

Services communicate primarily through gRPC, with protocol buffer definitions defining the interfaces. The scheduler service, for example, provides a `ScheduleMeet` endpoint that can be called by the chat backend.

## Technology Stack

- **Languages**: Python
- **Communication**: gRPC
- **Task Processing**: Celery
- **Message Broker**: Redis
- **Database**: MongoDB
- **ODM**: Beanie (for MongoDB)
- **Web Framework**: Not explicitly stated, likely FastAPI or Flask
- **Email**: SMTP for sending notifications

## Development and Extension

When developing or extending functionality:

1. Ensure gRPC protocol buffers are synchronized across services
2. Update Celery task definitions when adding new asynchronous operations
3. Maintain MongoDB schema compatibility when changing data models
4. Consider the distributed nature of the system when making changes
