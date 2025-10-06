<p align="center">
  <img src="https://github.com/Upstra/.github/blob/dcd1f2dc99276f0fd22eea7b8dd7f35902c562cc/PA2025%20Upstra%20Logo.png" alt="Upstra Logo" width="200"/>
</p>

<h1 align="center">Upstra - Infrastructure Control Platform</h1>

**Upstra** is a comprehensive, local-first infrastructure management platform designed for laboratories, research centers, and organizations requiring complete independence from cloud-based services. It provides enterprise-grade infrastructure control with a focus on power management, server orchestration, and operational resilience.

You can watch the demo here ([Youtube video](https://youtu.be/9E65qXEqmWo?si=W00yr7inuIbUVGSc))

## 🎯 Overview

Upstra is a full-stack infrastructure management solution that seamlessly integrates physical hardware control with virtual infrastructure management. The platform consists of three main components working in harmony:

1. **Python UPS Manager** - Hardware control and power management
2. **NestJS Backend** - Enterprise API and business logic
3. **Vue 3 Frontend** - Modern, reactive user interface

## 🏗️ Architecture

### System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                           Frontend (Vue 3)                           │
│  - Real-time Dashboard    - Command Palette    - Setup Wizard       │
│  - Infrastructure Tree    - SSH Terminal       - 2FA Auth           │
└─────────────────────────────────┬───────────────────────────────────┘
                                  │ HTTPS/WSS
┌─────────────────────────────────┴───────────────────────────────────┐
│                         Backend (NestJS)                             │
│  - RESTful API           - WebSocket Server   - Auth & Security     │
│  - Domain-Driven Design  - Event History      - Rate Limiting       │
└────────────┬────────────────────────────────────┬───────────────────┘
             │                                    │
┌────────────┴────────────┐          ┌───────────┴───────────────────┐
│   Python UPS Manager    │          │        PostgreSQL             │
│  - Power Management     │          │  - Encrypted Storage          │
│  - VM Migration         │          │  - Event Sourcing             │
│  - iLO Control         │          │  - Audit Trail                │
└─────────────────────────┘          └───────────────────────────────┘
             │                                    │
┌────────────┴────────────┐          ┌───────────┴───────────────────┐
│   Hardware Layer        │          │        Redis                  │
│  - HP iLO              │          │  - Session Management         │
│  - VMware vSphere      │          │  - Event Queue               │
│  - UPS Systems         │          │  - Real-time Presence        │
└─────────────────────────┘          └───────────────────────────────┘
```

## 🐍 Python UPS Manager

The UPS Manager is a sophisticated Python-based microservice that handles critical power management operations. For detailed information, see [ups_manager/README.md](ups_manager/README.md).

### Core Capabilities

- **Power Outage Response**: Automated VM migration and graceful server shutdown
- **Hardware Control**: Direct integration with HP iLO for physical server management
- **VMware Integration**: Full vSphere API support for VM lifecycle management
- **Event-Driven Architecture**: Redis-based event queue for state management
- **Rollback Support**: Complete system state restoration after power recovery

### Key Components

```python
data_retriever/
├── dto.py              # Data Transfer Objects and formatters
├── event_queue.py      # Redis event queue management
├── ilo.py              # HP iLO REST API integration
├── migration_event.py  # Event models for state tracking
├── vm_ware_connection.py # vSphere API wrapper
└── yaml_parser.py      # Configuration management
```

### Migration Workflow

1. **Grace Period**: Configurable delay before action (power might return)
2. **VM Migration**: Intelligent VM relocation to available hosts
3. **Server Shutdown**: Controlled power-off via iLO
4. **State Persistence**: All actions recorded in Redis for rollback
5. **Recovery**: Automatic restoration when power returns

## 🚀 Backend Architecture (NestJS)

The backend implements enterprise-grade patterns with a focus on security, scalability, and maintainability. For detailed documentation, see [infra-control/README.md](infra-control/README.md).

### Architecture Patterns

- **Domain-Driven Design (DDD)**: Clear separation of business logic
- **Hexagonal Architecture**: Ports and adapters for flexibility
- **Feature-First Modules**: Self-contained, loosely coupled modules

### Security Implementation

#### Multi-Layer Security
1. **Authentication**
   - JWT tokens with refresh mechanism
   - Two-Factor Authentication (TOTP)
   - Recovery codes for 2FA backup
   - Password reset with secure tokens

2. **Authorization**
   - Role-Based Access Control (RBAC)
   - Permission bitmasks for granular control
   - Resource-level permissions
   - Dynamic permission strategies

3. **Rate Limiting** (4 Tiers)
   - Global: 1000 requests/15 minutes
   - Authentication: 5 attempts/15 minutes
   - Sensitive Operations: 3 operations/hour
   - API Usage: 100 requests/5 minutes

4. **Additional Security**
   - Helmet.js security headers
   - CORS with whitelist
   - Database encryption (AES)
   - IP whitelisting

### Core Modules

```typescript
modules/
├── auth/          # JWT, 2FA, authentication flows
├── servers/       # Physical server management
├── vms/           # Virtual machine control
├── ups/           # UPS device management
├── rooms/         # Physical location tracking
├── users/         # User management
├── permissions/   # Granular access control
├── history/       # Audit trail and events
├── dashboard/     # Real-time metrics
├── presence/      # WebSocket user tracking
└── vmware/        # vSphere integration
```

### Database Architecture

- **PostgreSQL** with TypeORM
- **Migrations**: Automatic schema management
- **Encryption**: Sensitive field protection
- **Audit Trail**: Comprehensive event logging
- **Relations**: Complex entity relationships

### API Design

- **RESTful** endpoints with OpenAPI documentation
- **DTOs** with class-validator
- **Consistent** error responses
- **Paginated** results
- **Real-time** WebSocket support

## 🎨 Frontend Architecture (Vue 3)

The frontend provides a modern, reactive interface with enterprise features. For detailed information, see [infra-control_front/README.md](infra-control_front/README.md).

### Technology Stack

- **Vue 3.5** with Composition API
- **TypeScript** with strict mode
- **Vite** for blazing-fast builds
- **Tailwind CSS** for utility-first styling
- **Pinia** for state management
- **Vue Router** with guards
- **Element Plus** component library

### Architecture

```
src/
├── features/       # Feature-based modules
│   ├── auth/      # Authentication & 2FA
│   ├── dashboard/ # Real-time metrics
│   ├── servers/   # Server management
│   ├── setup/     # Initial configuration
│   └── ...        # Other features
├── shared/        # Reusable components
├── layouts/       # Application layouts
├── composables/   # Vue composables
└── store/         # Global state management
```

### Key Features

1. **Command Palette** (Cmd+K)
   - Global search and navigation
   - Context-aware actions
   - Admin-specific commands

2. **Real-time Updates**
   - WebSocket integration
   - User presence tracking
   - Live infrastructure status

3. **Advanced UI Components**
   - SSH Terminal (xterm.js)
   - Interactive dashboards
   - Drag-and-drop interfaces
   - Visual infrastructure builder

4. **Internationalization**
   - English and French support
   - Dynamic locale switching
   - Complete UI translation

## 🛠️ Best Practices

### Development Standards

1. **Code Organization**
   - Feature-first architecture
   - Clear separation of concerns
   - Consistent naming conventions
   - Comprehensive TypeScript types

2. **Testing Strategy**
   - Unit tests for business logic
   - E2E tests for critical flows
   - 80% coverage requirement
   - Mock factories for testing

3. **Security Practices**
   - No hardcoded credentials
   - Environment-based config
   - Encrypted sensitive data
   - Comprehensive validation

4. **Performance Optimization**
   - Database connection pooling
   - Redis caching strategies
   - Efficient query patterns
   - Code splitting (frontend)

### Operational Excellence

1. **Monitoring & Observability**
   - Prometheus metrics
   - Grafana dashboards
   - Structured logging
   - Performance tracking

2. **Deployment**
   - Docker containerization
   - Docker Compose orchestration
   - Environment-specific configs
   - Health check endpoints

3. **Documentation**
   - OpenAPI/Swagger specs
   - Inline code documentation
   - Architecture decision records
   - Setup and deployment guides

## 🚀 Future Enhancements

### Admin Parameter System
- Dynamic system configuration
- Runtime parameter updates
- Feature toggles
- Performance tuning controls

### Enhanced User Settings
- Customizable dashboards
- Personal preferences API
- Theme customization
- Notification preferences

### Push Notification System
- Real-time alerts
- WebSocket-based notifications
- Email integration
- SMS support (future)
- Custom notification rules

### Advanced Monitoring
- Custom metric definitions
- Alert rule configuration
- Predictive analytics
- Capacity planning tools

## 🏗️ Installation & Deployment

### Prerequisites
- Docker & Docker Compose
- Node.js 20+ (for development)
- Python 3.8+ (for UPS Manager)
- PostgreSQL 14+
- Redis 6+

### Quick Start
```bash
# Clone the repository
git clone https://github.com/Upstra/infra-control.git

# Start all services
docker-compose up -d

# Access the application
# Frontend: http://localhost:5173
# Backend API: http://localhost:3000
# API Docs: http://localhost:3000/docs
```

For detailed setup instructions, refer to the individual component READMEs.

## 🔐 Security Considerations

Upstra is designed with security at its core:
- All sensitive data is encrypted at rest
- Communication uses HTTPS/WSS
- Multi-factor authentication support
- Granular permission system
- Comprehensive audit logging
- Rate limiting on all endpoints

## 🤝 Contributing

We welcome contributions! Please see our contributing guidelines and code of conduct in the respective repositories.

---

Built with ❤️ for organizations that value independence, security, and operational excellence.

