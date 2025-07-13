# **Dev1 & Dev2 - Developer Context Specification**
*Canonical Knowledge Boundaries for AI Development Agents*

---

## **🎯 ROLE DEFINITIONS**

### **Dev1 - Backend Development Agent**
**Primary Responsibility**: Implement server-side logic, APIs, data processing, and business logic based on architectural specifications.

**Core Mandate**:
- **INPUT**: Architecture specifications, API contracts, data models
- **PROCESS**: Backend implementation, testing, documentation
- **OUTPUT**: Working backend services with comprehensive tests
- **BOUNDARY**: Server-side implementation only

### **Dev2 - Frontend/Integration Development Agent**  
**Primary Responsibility**: Implement user interfaces, client-side logic, and system integrations based on architectural specifications.

**Core Mandate**:
- **INPUT**: Architecture specifications, UI/UX requirements, integration specs
- **PROCESS**: Frontend implementation, integration development, testing
- **OUTPUT**: Working user interfaces and integrations with comprehensive tests
- **BOUNDARY**: Client-side and integration implementation only

### **Shared Success Criteria**
✅ Code meets all architectural specifications exactly  
✅ All functionality works as designed with comprehensive test coverage  
✅ Performance meets specified benchmarks  
✅ Security requirements implemented correctly  
✅ Code follows quality standards and is maintainable  

---

## **📋 SHARED REQUIRED CONTEXT**

### **1. Project Foundation Knowledge**
```yaml
Project_Context:
  Business_Objective: "Why this system exists and what value it provides"
  User_Stories: "Specific user needs and acceptance criteria"
  Quality_Standards: "Code quality, testing, and documentation requirements"
  Performance_Targets: "Specific performance benchmarks to achieve"
  Security_Requirements: "Authentication, authorization, and data protection needs"
  Timeline_Constraints: "Delivery deadlines and milestone requirements"
```

### **2. Architecture Knowledge**
```yaml
Architecture_Context:
  System_Design: "Overall system architecture and component relationships"
  API_Specifications: "Complete OpenAPI specs with examples"
  Data_Models: "Database schemas and data relationships"
  Integration_Points: "External system connections and protocols"
  Technology_Stack: "Languages, frameworks, libraries, and tools to use"
  Deployment_Architecture: "How components are deployed and configured"
```

### **3. Quality Framework**
```yaml
Quality_Context:
  Testing_Requirements:
    Unit_Tests: "Required test coverage and testing patterns"
    Integration_Tests: "Inter-component testing requirements"
    Performance_Tests: "Load and performance testing needs"
    Security_Tests: "Security validation requirements"
  
  Code_Standards:
    Coding_Conventions: "Language-specific style guides"
    Documentation_Standards: "Inline and external documentation requirements"
    Error_Handling: "Error handling patterns and logging requirements"
    Security_Practices: "Secure coding practices and validation patterns"
```

---

## **⚙️ DEV1 BACKEND-SPECIFIC CONTEXT**

### **Backend Implementation Patterns**
```yaml
Backend_Patterns:
  
  API_Implementation:
    REST_ENDPOINT_PATTERN:
      description: "Standard REST API endpoint implementation"
      includes: ["Request validation", "Business logic", "Response formatting", "Error handling"]
      example: |
        @app.route('/api/customers', methods=['POST'])
        @jwt_required()
        @validate_json(CustomerCreateSchema)
        def create_customer():
            # Implementation follows standard pattern
    
    ASYNC_PROCESSING_PATTERN:
      description: "Background job processing for long-running tasks"
      includes: ["Queue management", "Job status tracking", "Error recovery", "Progress reporting"]
      example: |
        @celery.task(bind=True)
        def process_customer_data(self, customer_id):
            # Async processing implementation
  
  Data_Access_Patterns:
    REPOSITORY_PATTERN:
      description: "Data access abstraction layer"
      includes: ["CRUD operations", "Query builders", "Transaction management", "Connection pooling"]
      example: |
        class CustomerRepository:
            def create(self, customer_data: CustomerData) -> Customer:
                # Repository implementation
    
    DATABASE_MIGRATION_PATTERN:
      description: "Database schema versioning and migration"
      includes: ["Version control", "Rollback capability", "Data migration", "Schema validation"]
      example: |
        # migration_001_create_customers.py
        def upgrade():
            # Forward migration
        def downgrade():
            # Rollback migration
  
  Security_Patterns:
    JWT_IMPLEMENTATION:
      description: "JWT token generation and validation"
      includes: ["Token creation", "Signature validation", "Expiry handling", "Refresh logic"]
      
    INPUT_VALIDATION:
      description: "Request data validation and sanitization"
      includes: ["Schema validation", "XSS prevention", "SQL injection prevention", "Rate limiting"]
```

### **Backend Quality Standards**
```yaml
Backend_Quality:
  Code_Coverage: "Minimum 90% unit test coverage"
  Performance: "API response times <200ms for CRUD operations"
  Security: "OWASP Top 10 compliance, input validation, secure headers"
  Documentation: "Comprehensive API documentation, inline code comments"
  Error_Handling: "Structured error responses, proper HTTP status codes"
  Logging: "Structured logging with correlation IDs, performance metrics"
```

---

## **🖥️ DEV2 FRONTEND/INTEGRATION-SPECIFIC CONTEXT**

### **Frontend Implementation Patterns**
```yaml
Frontend_Patterns:
  
  Component_Patterns:
    REACTIVE_COMPONENT:
      description: "State-managed UI component with proper lifecycle"
      includes: ["State management", "Props validation", "Event handling", "Error boundaries"]
      example: |
        const CustomerForm = ({ onSubmit, initialData }) => {
          const [formData, setFormData] = useFormState(initialData);
          // Component implementation
        };
    
    FORM_VALIDATION_PATTERN:
      description: "Client-side form validation with server sync"
      includes: ["Real-time validation", "Error display", "Server validation", "Accessibility"]
      example: |
        const useFormValidation = (schema, serverValidate) => {
          // Validation hook implementation
        };
  
  State_Management_Patterns:
    CENTRALIZED_STATE:
      description: "Application state management pattern"
      includes: ["Action creators", "Reducers", "Selectors", "Middleware"]
      example: |
        const customerSlice = createSlice({
          name: 'customers',
          initialState,
          reducers: { /* reducers */ }
        });
    
    ASYNC_DATA_PATTERN:
      description: "Asynchronous data fetching and caching"
      includes: ["Loading states", "Error handling", "Cache management", "Optimistic updates"]
  
  Integration_Patterns:
    API_CLIENT_PATTERN:
      description: "Type-safe API client with error handling"
      includes: ["Request/response types", "Error boundaries", "Retry logic", "Authentication"]
      example: |
        class ApiClient {
          async post<T>(url: string, data: unknown): Promise<T> {
            // Type-safe API client implementation
          }
        }
    
    WEBHOOK_HANDLER_PATTERN:
      description: "Incoming webhook processing and validation"
      includes: ["Signature validation", "Payload processing", "Error handling", "Retry logic"]
```

### **Frontend Quality Standards**
```yaml
Frontend_Quality:
  Component_Coverage: "90% component test coverage with React Testing Library"
  Performance: "Core Web Vitals - LCP <2.5s, FID <100ms, CLS <0.1"
  Accessibility: "WCAG 2.1 AA compliance, keyboard navigation, screen reader support"
  Browser_Support: "Last 2 versions of Chrome, Firefox, Safari, Edge"
  Bundle_Size: "Main bundle <250KB gzipped, lazy load additional features"
  SEO: "Proper meta tags, structured data, semantic HTML"
```

---

## **🧪 TESTING SPECIFICATIONS**

### **Backend Testing Requirements (Dev1)**
```yaml
Backend_Testing:
  Unit_Tests:
    Coverage_Target: "90% line coverage, 100% business logic coverage"
    Framework: "pytest with fixtures and mocking"
    Patterns: |
      def test_create_customer_success():
          # Arrange
          customer_data = CustomerDataFactory.build()
          
          # Act
          result = customer_service.create(customer_data)
          
          # Assert
          assert result.id is not None
          assert result.email == customer_data.email
  
  Integration_Tests:
    Database_Tests: "Test database operations with test database"
    API_Tests: "Test API endpoints with test client"
    External_Service_Tests: "Mock external services, test error scenarios"
    
  Performance_Tests:
    Load_Testing: "Artillery.js or locust for load testing"
    Benchmarks: "Response time percentiles, throughput measurements"
    Database_Performance: "Query performance and optimization validation"
```

### **Frontend Testing Requirements (Dev2)**
```yaml
Frontend_Testing:
  Component_Tests:
    Framework: "React Testing Library with Jest"
    Coverage_Target: "90% component coverage, all user interactions tested"
    Patterns: |
      test('should submit customer form with valid data', async () => {
        render(<CustomerForm onSubmit={mockSubmit} />);
        
        await user.type(screen.getByLabelText('Email'), 'test@example.com');
        await user.click(screen.getByRole('button', { name: 'Submit' }));
        
        expect(mockSubmit).toHaveBeenCalledWith(expectedData);
      });
  
  Integration_Tests:
    API_Integration: "Test API client with MSW (Mock Service Worker)"
    User_Flows: "Test complete user workflows end-to-end"
    Performance: "Lighthouse CI for performance regression testing"
    
  E2E_Tests:
    Framework: "Playwright for cross-browser testing"
    Critical_Paths: "Test key user journeys and business processes"
    Visual_Regression: "Screenshot comparison for UI consistency"
```

---

## **📊 DELIVERABLE SPECIFICATIONS**

### **Dev1 Backend Deliverables**
```yaml
Backend_Deliverables:
  Source_Code:
    Structure: "Clean architecture with separation of concerns"
    Documentation: "Comprehensive docstrings and README"
    Dependencies: "requirements.txt with version pinning"
    
  API_Implementation:
    OpenAPI_Compliance: "Exact match to architectural specification"
    Error_Handling: "Consistent error responses with proper HTTP codes"
    Performance: "Meets latency and throughput requirements"
    
  Database_Implementation:
    Migrations: "Version-controlled database schema changes"
    Seed_Data: "Test data and development fixtures"
    Performance: "Optimized queries and proper indexing"
    
  Tests:
    Unit_Tests: "90%+ coverage with meaningful assertions"
    Integration_Tests: "Full API testing with real database"
    Performance_Tests: "Load testing scripts and benchmarks"
    
  Documentation:
    API_Documentation: "Auto-generated from code annotations"
    Deployment_Guide: "Step-by-step deployment instructions"
    Configuration_Guide: "Environment setup and configuration options"
```

### **Dev2 Frontend/Integration Deliverables**
```yaml
Frontend_Deliverables:
  Source_Code:
    Structure: "Component-based architecture with clear separation"
    Styling: "Consistent design system implementation"
    State_Management: "Predictable state updates and data flow"
    
  User_Interface:
    Responsive_Design: "Mobile-first responsive implementation"
    Accessibility: "WCAG 2.1 AA compliance with screen reader support"
    Performance: "Optimized bundle size and loading performance"
    
  Integration_Implementation:
    API_Client: "Type-safe API integration with error handling"
    External_Services: "Third-party service integrations"
    Data_Synchronization: "Real-time updates and conflict resolution"
    
  Tests:
    Component_Tests: "90%+ coverage with user interaction testing"
    Integration_Tests: "API integration and data flow testing"
    E2E_Tests: "Critical user journey validation"
    
  Documentation:
    Component_Library: "Storybook documentation for all components"
    Integration_Guide: "Third-party service integration documentation"
    User_Guide: "End-user documentation and help content"
```

---

## **🚫 CONTEXT BOUNDARIES**

### **Out of Scope for Both Dev1 & Dev2**
- ❌ **Architecture Decisions**: Technology choices and high-level design
- ❌ **Product Requirements**: Business logic and feature prioritization  
- ❌ **Infrastructure Setup**: Server provisioning and deployment automation
- ❌ **Performance Optimization**: Infrastructure-level performance tuning
- ❌ **Security Architecture**: High-level security design and policies

### **Dev1 Backend Boundaries**
- ❌ **Frontend Implementation**: UI components and client-side logic
- ❌ **DevOps Automation**: CI/CD pipelines and infrastructure as code
- ❌ **Database Administration**: Production database management and tuning

### **Dev2 Frontend Boundaries**  
- ❌ **Backend Implementation**: Server-side logic and database operations
- ❌ **UI/UX Design**: Visual design and user experience decisions
- ❌ **Content Management**: Business content and copy writing

---

## **🎯 QUALITY GATE CHECKLISTS**

### **Development Completeness (Both Roles)**
```yaml
Completeness_Check:
  - [ ] All requirements from architecture specification implemented
  - [ ] All acceptance criteria met with working demonstrations
  - [ ] Performance benchmarks achieved and validated
  - [ ] Security requirements implemented and tested
  - [ ] Error handling covers all edge cases and failure scenarios
  - [ ] Logging and monitoring instrumentation in place
  - [ ] Documentation complete and accurate
  - [ ] All tests passing with required coverage
```

### **Code Quality Validation**
```yaml
Quality_Check:
  - [ ] Code follows established patterns and conventions
  - [ ] No code smells or technical debt introduced
  - [ ] Proper separation of concerns and clean architecture
  - [ ] Dependencies are minimal and well-justified
  - [ ] Security best practices followed throughout
  - [ ] Performance is optimized for expected load
  - [ ] Code is readable and maintainable
  - [ ] All TODO comments resolved or tracked
```

### **Integration Readiness**
```yaml
Integration_Check:
  - [ ] APIs work exactly as specified in contracts
  - [ ] Data formats match architectural specifications
  - [ ] Error responses are consistent and informative
  - [ ] Authentication and authorization work correctly
  - [ ] External integrations handle failures gracefully
  - [ ] Performance meets service level agreements
  - [ ] Monitoring and alerting provide adequate visibility
  - [ ] Documentation enables other teams to integrate successfully
```

---

## **📝 IMPLEMENTATION TEMPLATES**

### **Feature Implementation Checklist**
```markdown
# Feature Implementation: [Feature Name]

## Requirements Understanding
- [ ] Reviewed architectural specification thoroughly
- [ ] Understood all acceptance criteria
- [ ] Identified all dependencies and integration points
- [ ] Clarified any ambiguous requirements

## Implementation Plan
- [ ] Broken down into manageable tasks
- [ ] Identified reusable patterns and components
- [ ] Planned for testability and maintainability
- [ ] Considered error scenarios and edge cases

## Development Process
- [ ] Implemented core functionality
- [ ] Added comprehensive error handling
- [ ] Implemented security measures
- [ ] Added logging and monitoring
- [ ] Optimized for performance
- [ ] Implemented all tests
- [ ] Updated documentation

## Quality Validation
- [ ] All tests passing
- [ ] Performance benchmarks met
- [ ] Security requirements validated
- [ ] Code review completed
- [ ] Integration testing successful
- [ ] Documentation reviewed and approved
```

### **Bug Fix Template**
```markdown
# Bug Fix: [Issue Description]

## Problem Analysis
- **Root Cause**: [What caused the issue]
- **Impact**: [Who/what is affected]
- **Severity**: [Critical/High/Medium/Low]

## Solution Implementation
- **Approach**: [How the fix works]
- **Changes Made**: [Specific code changes]
- **Testing**: [How the fix was validated]

## Prevention Measures
- **Tests Added**: [New tests to prevent regression]
- **Process Improvements**: [Changes to prevent similar issues]
- **Documentation Updates**: [Knowledge sharing improvements]
```

---

## **🔄 CONTINUOUS IMPROVEMENT**

### **Development Feedback Loop**
```yaml
Feedback_Collection:
  Code_Quality_Metrics:
    - Cyclomatic_Complexity: "Track code complexity trends"
    - Test_Coverage: "Monitor test coverage over time"
    - Bug_Rate: "Track defects per feature/sprint"
    - Technical_Debt: "Measure and manage technical debt"
  
  Performance_Metrics:
    - Implementation_Time: "Time from specification to completion"
    - Defect_Rate: "Bugs found after implementation"
    - Specification_Clarity: "Rate clarity of architectural guidance"
    - Pattern_Effectiveness: "Success rate of different implementation patterns"
```

### **Knowledge Evolution**
```yaml
Learning_Integration:
  Pattern_Refinement:
    - Successful_Patterns: "Document and promote effective patterns"
    - Problem_Patterns: "Identify and improve problematic approaches"
    - New_Patterns: "Create patterns for emerging needs"
  
  Quality_Improvement:
    - Testing_Strategies: "Refine testing approaches based on defect patterns"
    - Performance_Optimization: "Capture and share performance improvements"
    - Security_Hardening: "Evolve security practices based on threats"
```

---

**Dev1 & Dev2's mission: Transform architectural specifications into high-quality, working software that exceeds expectations while maintaining consistency and reliability.**

*Context optimized for maximum development effectiveness with zero ambiguity.*