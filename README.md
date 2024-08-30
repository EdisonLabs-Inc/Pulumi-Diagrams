# Pulumi-Diagrams

Mermaid diagrams from CodeViz for the Pulumi repo. These diagrams are nested and link to source code in the extension, but hope this is still useful for understanding.

### Level 1: System Architecture
```mermaid
graph TB
    User((User))
    
    subgraph "Pulumi System"
        CLI["CLI<br/>(Go)"]
        
        subgraph "SDKs"
            GoSDK["Go SDK"]
            NodeSDK["Node.js SDK"]
            PythonSDK["Python SDK"]
        end
        
        subgraph "Core Components"
            Engine["Engine<br/>(Go)"]
            ResourceManager["Resource Manager<br/>(Go)"]
            StateManager["State Manager<br/>(Go)"]
            ConfigManager["Config Manager<br/>(Go)"]
        end
        
        subgraph "Plugins"
            ProviderPlugins["Provider Plugins<br/>(Various Languages)"]
            LanguagePlugins["Language Plugins<br/>(Various Languages)"]
        end
        
        subgraph "Backend Services"
            APIServer["API Server<br/>(Go)"]
            StorageService["Storage Service<br/>(Go)"]
        end
        
        subgraph "Data Storage"
            StateStorage[("State Storage<br/>(e.g., S3, Azure Blob)")]
        end
    end
    
    subgraph "External Systems"
        CloudProviders["Cloud Providers<br/>(AWS, Azure, GCP, etc.)"]
        VersionControl["Version Control<br/>(e.g., GitHub, GitLab)"]
    end
    
    User --> CLI
    CLI --> GoSDK
    CLI --> NodeSDK
    CLI --> PythonSDK
    
    CLI --> Engine
    Engine --> ResourceManager
    Engine --> StateManager
    Engine --> ConfigManager
    
    ResourceManager --> ProviderPlugins
    Engine --> LanguagePlugins
    
    Engine --> APIServer
    StateManager --> StorageService
    StorageService --> StateStorage
    
    ProviderPlugins --> CloudProviders
    CLI --> VersionControl
    
    classDef cli fill:#1168bd,stroke:#0b4884,color:#ffffff
    classDef sdk fill:#2694ab,stroke:#1a6d7d,color:#ffffff
    classDef core fill:#2b78e4,stroke:#1a4d91,color:#ffffff
    classDef plugins fill:#6b8e23,stroke:#556b2f,color:#ffffff
    classDef backend fill:#ff7f50,stroke:#d2691e,color:#ffffff
    classDef storage fill:#8a2be2,stroke:#4b0082,color:#ffffff
    classDef external fill:#999999,stroke:#666666,color:#ffffff
    
    class CLI cli
    class GoSDK,NodeSDK,PythonSDK sdk
    class Engine,ResourceManager,StateManager,ConfigManager core
    class ProviderPlugins,LanguagePlugins plugins
    class APIServer,StorageService backend
    class StateStorage storage
    class CloudProviders,VersionControl external
```

### Level 2: CLI
```mermaid
graph TD
    User((User))
    
    subgraph "CLI Container"
        MainEntryPoint[Main Entry Point]
        
        subgraph "Core Components"
            CommandHandler[Command Handler]
            ConfigManager[Config Manager]
            StackManager[Stack Manager]
            ResourceManager[Resource Manager]
            PluginManager[Plugin Manager]
        end
        
        subgraph "Command Implementations"
            NewCmd[New Command]
            UpCmd[Up Command]
            PreviewCmd[Preview Command]
            DestroyCmd[Destroy Command]
            ConfigCmd[Config Command]
            StackCmd[Stack Command]
        end
        
        subgraph "Utility Components"
            Logger[Logger]
            ErrorHandler[Error Handler]
            PromptManager[Prompt Manager]
            OutputFormatter[Output Formatter]
        end
        
        subgraph "Backend Interaction"
            BackendClient[Backend Client]
            SecretsManager[Secrets Manager]
        end
        
        subgraph "Project Management"
            ProjectLoader[Project Loader]
            TemplateManager[Template Manager]
        end
    end
    
    Backend[(Backend Services)]
    FileSystem[(File System)]
    
    User -->|interacts with| MainEntryPoint
    MainEntryPoint --> CommandHandler
    CommandHandler --> NewCmd
    CommandHandler --> UpCmd
    CommandHandler --> PreviewCmd
    CommandHandler --> DestroyCmd
    CommandHandler --> ConfigCmd
    CommandHandler --> StackCmd
    
    NewCmd --> ProjectLoader
    NewCmd --> TemplateManager
    UpCmd --> ResourceManager
    UpCmd --> StackManager
    PreviewCmd --> ResourceManager
    DestroyCmd --> ResourceManager
    ConfigCmd --> ConfigManager
    StackCmd --> StackManager
    
    ResourceManager --> PluginManager
    StackManager --> BackendClient
    ConfigManager --> SecretsManager
    
    CommandHandler -.->|uses| Logger
    CommandHandler -.->|uses| ErrorHandler
    CommandHandler -.->|uses| PromptManager
    CommandHandler -.->|uses| OutputFormatter
    
    BackendClient --> Backend
    ProjectLoader --> FileSystem
    TemplateManager --> FileSystem
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef command fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef utility fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef backend fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef project fill:#9b59b6,stroke:#8e44ad,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class MainEntryPoint,CommandHandler,ConfigManager,StackManager,ResourceManager,PluginManager core
    class NewCmd,UpCmd,PreviewCmd,DestroyCmd,ConfigCmd,StackCmd command
    class Logger,ErrorHandler,PromptManager,OutputFormatter utility
    class BackendClient,SecretsManager backend
    class ProjectLoader,TemplateManager project
    class User,Backend,FileSystem external
```

### Level 2: Backend
#### Api Server
```mermaid
graph TD
    subgraph "API Server"
        APIGateway[API Gateway]
        
        subgraph "Core Components"
            RequestHandler[Request Handler]
            ResponseFormatter[Response Formatter]
            ErrorHandler[Error Handler]
            Middleware[Middleware]
        end
        
        subgraph "Authentication"
            AuthManager[Auth Manager]
            TokenValidator[Token Validator]
        end
        
        subgraph "Resource Management"
            ResourceManager[Resource Manager]
            StackManager[Stack Manager]
            DeploymentManager[Deployment Manager]
        end
        
        subgraph "State Management"
            StateManager[State Manager]
            CheckpointManager[Checkpoint Manager]
        end
        
        subgraph "Event Handling"
            EventDispatcher[Event Dispatcher]
            EventLogger[Event Logger]
        end
        
        subgraph "Data Access"
            DataAccessLayer[Data Access Layer]
            QueryBuilder[Query Builder]
        end
        
        subgraph "External Integrations"
            CloudProviderClient[Cloud Provider Client]
            SecretsManager[Secrets Manager]
        end
        
        ConfigManager[Config Manager]
        Logger[Logger]
    end
    
    Database[(Database)]
    MessageQueue[Message Queue]
    
    APIGateway --> RequestHandler
    RequestHandler --> Middleware
    Middleware --> AuthManager
    AuthManager --> TokenValidator
    RequestHandler --> ResourceManager
    ResourceManager --> StackManager
    ResourceManager --> DeploymentManager
    StackManager --> StateManager
    DeploymentManager --> CheckpointManager
    StateManager --> DataAccessLayer
    CheckpointManager --> DataAccessLayer
    DataAccessLayer --> QueryBuilder
    QueryBuilder --> Database
    
    RequestHandler --> EventDispatcher
    EventDispatcher --> EventLogger
    EventLogger --> MessageQueue
    
    ResourceManager --> CloudProviderClient
    AuthManager --> SecretsManager
    
    RequestHandler --> ResponseFormatter
    RequestHandler --> ErrorHandler
    
    ResourceManager --> ConfigManager
    ResourceManager --> Logger
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef auth fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef resource fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef state fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef event fill:#9b59b6,stroke:#8e44ad,color:#ffffff
    classDef data fill:#1abc9c,stroke:#16a085,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class APIGateway,RequestHandler,ResponseFormatter,ErrorHandler,Middleware core
    class AuthManager,TokenValidator auth
    class ResourceManager,StackManager,DeploymentManager resource
    class StateManager,CheckpointManager state
    class EventDispatcher,EventLogger event
    class DataAccessLayer,QueryBuilder data
    class CloudProviderClient,SecretsManager,Database,MessageQueue external
    class ConfigManager,Logger external
```
#### Config Manager
```mermaid
graph TD
    subgraph "Config Manager"
        SecretManager[Secret Manager]
        ConfigProvider[Config Provider]
        EnvironmentManager[Environment Manager]
        ConfigValidator[Config Validator]
        ConfigParser[Config Parser]
        ConfigSerializer[Config Serializer]
        EncryptionService[Encryption Service]
        DecryptionService[Decryption Service]
        ConfigStore[Config Store]
        ConfigVersioning[Config Versioning]
        ConfigMerger[Config Merger]
        ConfigDiff[Config Diff]
    end

    SecretManager --> EncryptionService
    SecretManager --> DecryptionService
    ConfigProvider --> ConfigStore
    ConfigProvider --> ConfigParser
    EnvironmentManager --> ConfigStore
    ConfigValidator --> ConfigParser
    ConfigSerializer --> ConfigStore
    ConfigVersioning --> ConfigStore
    ConfigMerger --> ConfigDiff
    ConfigMerger --> ConfigStore

    classDef core fill:#2b78e4,stroke:#1a4d91,color:#ffffff
    classDef security fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef storage fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef utility fill:#f39c12,stroke:#d35400,color:#ffffff

    class SecretManager,EncryptionService,DecryptionService security
    class ConfigProvider,EnvironmentManager,ConfigStore core
    class ConfigValidator,ConfigParser,ConfigSerializer utility
    class ConfigVersioning,ConfigMerger,ConfigDiff storage
```
#### Engine
```mermaid
graph TD
    subgraph "Engine Container"
        DeploymentManager[Deployment Manager]
        UpdateManager[Update Manager]
        QueryManager[Query Manager]
        PluginManager[Plugin Manager]
        EventManager[Event Manager]
        ResourceManager[Resource Manager]
        ConfigManager[Config Manager]
        SnapshotManager[Snapshot Manager]
        
        subgraph "Core Components"
            DeploymentContext[Deployment Context]
            UpdateSource[Update Source]
            QuerySource[Query Source]
            ResourceOperations[Resource Operations]
        end
        
        subgraph "Utility Components"
            ProjectInfoContext[Project Info Context]
            EventEmitter[Event Emitter]
            DiagnosticHandler[Diagnostic Handler]
        end
        
        subgraph "Policy Components"
            PolicyManager[Policy Manager]
            PolicyAnalyzer[Policy Analyzer]
        end
    end
    
    CLI[CLI]
    Plugins[Plugins]
    BackendServices[Backend Services]
    
    CLI --> DeploymentManager
    CLI --> UpdateManager
    CLI --> QueryManager
    
    DeploymentManager --> DeploymentContext
    UpdateManager --> UpdateSource
    QueryManager --> QuerySource
    
    DeploymentManager --> PluginManager
    UpdateManager --> PluginManager
    QueryManager --> PluginManager
    
    PluginManager --> Plugins
    
    DeploymentManager --> EventManager
    UpdateManager --> EventManager
    QueryManager --> EventManager
    
    EventManager --> EventEmitter
    
    DeploymentManager --> ResourceManager
    UpdateManager --> ResourceManager
    QueryManager --> ResourceManager
    
    ResourceManager --> ResourceOperations
    
    DeploymentManager --> ConfigManager
    UpdateManager --> ConfigManager
    QueryManager --> ConfigManager
    
    DeploymentManager --> SnapshotManager
    UpdateManager --> SnapshotManager
    
    DeploymentManager --> ProjectInfoContext
    UpdateManager --> ProjectInfoContext
    QueryManager --> ProjectInfoContext
    
    EventManager --> DiagnosticHandler
    
    PolicyManager --> PolicyAnalyzer
    DeploymentManager --> PolicyManager
    UpdateManager --> PolicyManager
    
    DeploymentManager --> BackendServices
    UpdateManager --> BackendServices
    QueryManager --> BackendServices
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef utility fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef policy fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class DeploymentManager,UpdateManager,QueryManager,PluginManager,EventManager,ResourceManager,ConfigManager,SnapshotManager core
    class DeploymentContext,UpdateSource,QuerySource,ResourceOperations core
    class ProjectInfoContext,EventEmitter,DiagnosticHandler utility
    class PolicyManager,PolicyAnalyzer policy
    class CLI,Plugins,BackendServices external
```
#### Resource Manager
```mermaid
graph TD
    subgraph "Resource Manager"
        DeploymentManager[Deployment Manager]
        ResourceProvider[Resource Provider]
        StateManager[State Manager]
        ImportManager[Import Manager]
        ConfigValidator[Config Validator]
        
        subgraph "Core Components"
            StepGenerator[Step Generator]
            StepExecutor[Step Executor]
            DependencyGraph[Dependency Graph]
        end
        
        subgraph "Resource Handling"
            ResourceAnalyzer[Resource Analyzer]
            ResourceDiff[Resource Diff]
            ResourceCreator[Resource Creator]
            ResourceUpdater[Resource Updater]
            ResourceDeleter[Resource Deleter]
        end
        
        subgraph "Provider Management"
            ProviderRegistry[Provider Registry]
            ProviderLoader[Provider Loader]
            ProviderConfigurator[Provider Configurator]
        end
        
        subgraph "Utilities"
            URNGenerator[URN Generator]
            EventHandler[Event Handler]
            ErrorHandler[Error Handler]
        end
    end
    
    ExternalPlugins[External Plugins]
    StateStorage[(State Storage)]
    
    DeploymentManager --> StepGenerator
    DeploymentManager --> StepExecutor
    DeploymentManager --> DependencyGraph
    DeploymentManager --> ImportManager
    DeploymentManager --> ConfigValidator
    
    StepGenerator --> ResourceAnalyzer
    StepGenerator --> ResourceDiff
    StepExecutor --> ResourceCreator
    StepExecutor --> ResourceUpdater
    StepExecutor --> ResourceDeleter
    
    ResourceProvider --> ProviderRegistry
    ProviderRegistry --> ProviderLoader
    ProviderRegistry --> ProviderConfigurator
    
    DeploymentManager --> URNGenerator
    DeploymentManager --> EventHandler
    DeploymentManager --> ErrorHandler
    
    StateManager --> StateStorage
    ProviderLoader --> ExternalPlugins
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef resourceHandling fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef providerManagement fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef utilities fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class DeploymentManager,ResourceProvider,StateManager,ImportManager,ConfigValidator core
    class StepGenerator,StepExecutor,DependencyGraph core
    class ResourceAnalyzer,ResourceDiff,ResourceCreator,ResourceUpdater,ResourceDeleter resourceHandling
    class ProviderRegistry,ProviderLoader,ProviderConfigurator providerManagement
    class URNGenerator,EventHandler,ErrorHandler utilities
    class ExternalPlugins,StateStorage external
```
#### State Manager
```mermaid
graph TD
    subgraph "State Manager"
        SnapshotManager[Snapshot Manager]
        SerializationService[Serialization Service]
        DeserializationService[Deserialization Service]
        CheckpointManager[Checkpoint Manager]
        ResourceStateManager[Resource State Manager]
        SecretManager[Secret Manager]
        DeploymentManager[Deployment Manager]
        MigrationService[Migration Service]
        VersioningService[Versioning Service]
        IntegrityChecker[Integrity Checker]
        CachingService[Caching Service]
        
        subgraph "Storage Layer"
            SnapshotPersister[Snapshot Persister]
            InMemoryPersister[In-Memory Persister]
            DIYStore[DIY Store]
        end
        
        subgraph "Encryption"
            Encrypter[Encrypter]
            Decrypter[Decrypter]
        end
    end
    
    Engine[Engine]
    StorageService[Storage Service]
    
    Engine --> SnapshotManager
    SnapshotManager --> SerializationService
    SnapshotManager --> DeserializationService
    SnapshotManager --> CheckpointManager
    SnapshotManager --> ResourceStateManager
    SnapshotManager --> SecretManager
    SnapshotManager --> DeploymentManager
    SnapshotManager --> MigrationService
    SnapshotManager --> VersioningService
    SnapshotManager --> IntegrityChecker
    SnapshotManager --> CachingService
    
    SerializationService --> SnapshotPersister
    DeserializationService --> SnapshotPersister
    SnapshotPersister --> InMemoryPersister
    SnapshotPersister --> DIYStore
    
    SecretManager --> Encrypter
    SecretManager --> Decrypter
    
    SnapshotManager --> StorageService
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef storage fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef encryption fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class SnapshotManager,SerializationService,DeserializationService,CheckpointManager,ResourceStateManager,SecretManager,DeploymentManager,MigrationService,VersioningService,IntegrityChecker,CachingService core
    class SnapshotPersister,InMemoryPersister,DIYStore storage
    class Encrypter,Decrypter encryption
    class Engine,StorageService external
```
#### Storage Server
```mermaid
graph TD
    subgraph "Storage Service"
        Backend[Backend<br/>Go]
        
        subgraph "Core Components"
            DIYBackend[DIY Backend]
            BucketManager[Bucket Manager]
            ReferenceStore[Reference Store]
            SnapshotManager[Snapshot Manager]
        end
        
        subgraph "Storage Operations"
            StackOperations[Stack Operations]
            CheckpointManager[Checkpoint Manager]
            HistoryManager[History Manager]
            BackupManager[Backup Manager]
        end
        
        subgraph "Query and Update"
            QueryEngine[Query Engine]
            UpdateEngine[Update Engine]
        end
        
        subgraph "Security"
            AuthManager[Auth Manager]
            EncryptionService[Encryption Service]
        end
        
        subgraph "Infrastructure"
            ConfigManager[Config Manager]
            Logger[Logger]
        end
        
        APIClient[API Client]
    end
    
    ExternalStorage[(External Storage<br/>S3, Azure Blob, GCS)]
    
    Backend --> DIYBackend
    DIYBackend --> BucketManager
    DIYBackend --> ReferenceStore
    DIYBackend --> SnapshotManager
    
    DIYBackend --> StackOperations
    StackOperations --> CheckpointManager
    StackOperations --> HistoryManager
    StackOperations --> BackupManager
    
    DIYBackend --> QueryEngine
    DIYBackend --> UpdateEngine
    
    DIYBackend --> AuthManager
    DIYBackend --> EncryptionService
    
    DIYBackend --> ConfigManager
    DIYBackend --> Logger
    
    BucketManager --> ExternalStorage
    
    APIClient --> Backend
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef operations fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef query fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef security fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef infrastructure fill:#9b59b6,stroke:#8e44ad,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class Backend,DIYBackend,BucketManager,ReferenceStore,SnapshotManager core
    class StackOperations,CheckpointManager,HistoryManager,BackupManager operations
    class QueryEngine,UpdateEngine query
    class AuthManager,EncryptionService security
    class ConfigManager,Logger infrastructure
    class ExternalStorage,APIClient external
```

### Level 2: Shared Components
#### Go SDK
```mermaid
graph TD
    User((User))
    
    subgraph "Go SDK"
        RunFunc[Run Function]
        
        subgraph "Core Components"
            Context[Context]
            ResourceState[Resource State]
            OutputState[Output State]
            Config[Config Manager]
        end
        
        subgraph "Resource Management"
            CustomResource[Custom Resource]
            ComponentResource[Component Resource]
            ProviderResource[Provider Resource]
            ResourceOptions[Resource Options]
        end
        
        subgraph "Input/Output Handling"
            Input[Input]
            Output[Output]
            PropertyMap[Property Map]
        end
        
        subgraph "RPC Communication"
            RPCClient[RPC Client]
            Serialization[Serialization]
        end
        
        subgraph "Logging and Diagnostics"
            Logger[Logger]
            DiagnosticWriter[Diagnostic Writer]
        end
        
        subgraph "Workspace Management"
            Project[Project]
            Stack[Stack]
            PluginManager[Plugin Manager]
        end
        
        APIClient[API Client]
    end
    
    Backend[Pulumi Service]
    
    User -->|uses| RunFunc
    RunFunc --> Context
    Context --> ResourceState
    Context --> OutputState
    Context --> Config
    
    ResourceState --> CustomResource
    ResourceState --> ComponentResource
    ResourceState --> ProviderResource
    ResourceState --> ResourceOptions
    
    CustomResource --> Input
    CustomResource --> Output
    ComponentResource --> Input
    ComponentResource --> Output
    ProviderResource --> Input
    ProviderResource --> Output
    
    Input --> PropertyMap
    Output --> PropertyMap
    
    Context --> RPCClient
    RPCClient --> Serialization
    RPCClient --> Backend
    
    Context --> Logger
    Logger --> DiagnosticWriter
    
    Context --> Project
    Context --> Stack
    Context --> PluginManager
    
    APIClient --> Backend
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef resource fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef io fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef rpc fill:#9b59b6,stroke:#8e44ad,color:#ffffff
    classDef logging fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef workspace fill:#1abc9c,stroke:#16a085,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class Context,ResourceState,OutputState,Config core
    class CustomResource,ComponentResource,ProviderResource,ResourceOptions resource
    class Input,Output,PropertyMap io
    class RPCClient,Serialization rpc
    class Logger,DiagnosticWriter logging
    class Project,Stack,PluginManager workspace
    class User,Backend,APIClient external
```
#### Node.js SDK
```mermaid
graph TD
    subgraph "Node.js SDK"
        CoreModule[Core Module]
        ResourceModule[Resource Module]
        ConfigModule[Config Module]
        OutputModule[Output Module]
        ProviderModule[Provider Module]
        DynamicModule[Dynamic Module]
        AutomationModule[Automation Module]
        
        subgraph "Utility Components"
            ErrorHandler[Error Handler]
            Logger[Logger]
            TypescriptUtils[Typescript Utils]
        end
        
        subgraph "Runtime Components"
            ResourceManager[Resource Manager]
            StateManager[State Manager]
            ConfigManager[Config Manager]
            ClosureSerializer[Closure Serializer]
        end
        
        subgraph "API Components"
            ResourceAPI[Resource API]
            ConfigAPI[Config API]
            OutputAPI[Output API]
            ProviderAPI[Provider API]
        end
        
        subgraph "Serialization"
            JSONSerializer[JSON Serializer]
            ProtobufSerializer[Protobuf Serializer]
        end
        
        CLIIntegration[CLI Integration]
        EngineIntegration[Engine Integration]
    end
    
    PulumiEngine[Pulumi Engine]
    
    CoreModule --> ResourceModule
    CoreModule --> ConfigModule
    CoreModule --> OutputModule
    CoreModule --> ProviderModule
    CoreModule --> DynamicModule
    CoreModule --> AutomationModule
    
    ResourceModule --> ResourceManager
    ResourceModule --> ResourceAPI
    ConfigModule --> ConfigManager
    ConfigModule --> ConfigAPI
    OutputModule --> OutputAPI
    ProviderModule --> ProviderAPI
    
    ResourceManager --> StateManager
    ResourceManager --> ClosureSerializer
    
    CoreModule --> ErrorHandler
    CoreModule --> Logger
    CoreModule --> TypescriptUtils
    
    ResourceAPI --> JSONSerializer
    ResourceAPI --> ProtobufSerializer
    ConfigAPI --> JSONSerializer
    OutputAPI --> JSONSerializer
    ProviderAPI --> ProtobufSerializer
    
    CLIIntegration --> CoreModule
    EngineIntegration --> CoreModule
    EngineIntegration --> PulumiEngine
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef module fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef utility fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef runtime fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef api fill:#9b59b6,stroke:#8e44ad,color:#ffffff
    classDef serialization fill:#1abc9c,stroke:#16a085,color:#ffffff
    classDef integration fill:#34495e,stroke:#2c3e50,color:#ffffff
    classDef external fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    
    class CoreModule core
    class ResourceModule,ConfigModule,OutputModule,ProviderModule,DynamicModule,AutomationModule module
    class ErrorHandler,Logger,TypescriptUtils utility
    class ResourceManager,StateManager,ConfigManager,ClosureSerializer runtime
    class ResourceAPI,ConfigAPI,OutputAPI,ProviderAPI api
    class JSONSerializer,ProtobufSerializer serialization
    class CLIIntegration,EngineIntegration integration
    class PulumiEngine external
```
#### Provider Plugins
```mermaid
graph TD
    subgraph "Provider Plugins"
        ProviderManager[Provider Manager]
        PluginLoader[Plugin Loader]
        
        subgraph "Core Components"
            ProviderInterface[Provider Interface]
            ProviderServer[Provider Server]
            ProviderClient[Provider Client]
        end
        
        subgraph "Resource Operations"
            ResourceCreator[Resource Creator]
            ResourceReader[Resource Reader]
            ResourceUpdater[Resource Updater]
            ResourceDeleter[Resource Deleter]
            ResourceDiffer[Resource Differ]
        end
        
        subgraph "Schema Management"
            SchemaManager[Schema Manager]
            SchemaValidator[Schema Validator]
        end
        
        subgraph "Configuration"
            ConfigManager[Config Manager]
            ParameterManager[Parameter Manager]
        end
        
        subgraph "RPC Communication"
            GRPCServer[gRPC Server]
            RPCErrorHandler[RPC Error Handler]
        end
        
        subgraph "Utility Components"
            PropertyMarshaller[Property Marshaller]
            DiffCalculator[Diff Calculator]
            PluginInfoProvider[Plugin Info Provider]
        end
    end
    
    ProviderManager --> PluginLoader
    ProviderManager --> ProviderInterface
    ProviderServer --> ProviderInterface
    ProviderClient --> ProviderInterface
    
    ProviderInterface --> ResourceCreator
    ProviderInterface --> ResourceReader
    ProviderInterface --> ResourceUpdater
    ProviderInterface --> ResourceDeleter
    ProviderInterface --> ResourceDiffer
    
    ProviderServer --> SchemaManager
    SchemaManager --> SchemaValidator
    
    ProviderServer --> ConfigManager
    ConfigManager --> ParameterManager
    
    ProviderServer --> GRPCServer
    GRPCServer --> RPCErrorHandler
    
    ResourceCreator --> PropertyMarshaller
    ResourceReader --> PropertyMarshaller
    ResourceUpdater --> PropertyMarshaller
    ResourceDiffer --> DiffCalculator
    
    ProviderManager --> PluginInfoProvider

    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef resource fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef schema fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef config fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef rpc fill:#9b59b6,stroke:#8e44ad,color:#ffffff
    classDef utility fill:#1abc9c,stroke:#16a085,color:#ffffff
    
    class ProviderManager,PluginLoader,ProviderInterface,ProviderServer,ProviderClient core
    class ResourceCreator,ResourceReader,ResourceUpdater,ResourceDeleter,ResourceDiffer resource
    class SchemaManager,SchemaValidator schema
    class ConfigManager,ParameterManager config
    class GRPCServer,RPCErrorHandler rpc
    class PropertyMarshaller,DiffCalculator,PluginInfoProvider utility
```
#### Python SDK
```mermaid
graph TD
    User((User))

    subgraph "Python SDK"
        CoreModule[Core Module]
        
        subgraph "Resource Management"
            ResourceClass[Resource Class]
            CustomResource[Custom Resource]
            ComponentResource[Component Resource]
            ProviderResource[Provider Resource]
        end
        
        subgraph "Configuration"
            ConfigModule[Config Module]
            ConfigManager[Config Manager]
        end
        
        subgraph "Output Handling"
            OutputClass[Output Class]
            InputHandling[Input Handling]
        end
        
        subgraph "Runtime"
            ResourceRuntime[Resource Runtime]
            StackRuntime[Stack Runtime]
            Settings[Settings]
        end
        
        subgraph "Automation API"
            LocalWorkspace[Local Workspace]
            Stack[Stack]
            ProjectSettings[Project Settings]
        end
        
        subgraph "Dynamic Providers"
            DynamicProvider[Dynamic Provider]
            DynamicResource[Dynamic Resource]
        end
        
        subgraph "Utilities"
            Logging[Logging]
            AssetArchive[Asset & Archive]
        end
    end
    
    PulumiCLI[Pulumi CLI]
    ResourceProviders[Resource Providers]
    
    User --> PulumiCLI
    PulumiCLI --> CoreModule
    CoreModule --> ResourceManagement
    CoreModule --> Configuration
    CoreModule --> OutputHandling
    CoreModule --> Runtime
    CoreModule --> AutomationAPI
    CoreModule --> DynamicProviders
    CoreModule --> Utilities
    
    ResourceRuntime --> ResourceProviders
    
    classDef core fill:#3498db,stroke:#2980b9,color:#ffffff
    classDef resourceMgmt fill:#2ecc71,stroke:#27ae60,color:#ffffff
    classDef config fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef output fill:#9b59b6,stroke:#8e44ad,color:#ffffff
    classDef runtime fill:#f39c12,stroke:#d35400,color:#ffffff
    classDef automation fill:#1abc9c,stroke:#16a085,color:#ffffff
    classDef dynamic fill:#34495e,stroke:#2c3e50,color:#ffffff
    classDef utils fill:#95a5a6,stroke:#7f8c8d,color:#ffffff
    classDef external fill:#bdc3c7,stroke:#95a5a6,color:#333333
    
    class CoreModule core
    class ResourceClass,CustomResource,ComponentResource,ProviderResource resourceMgmt
    class ConfigModule,ConfigManager config
    class OutputClass,InputHandling output
    class ResourceRuntime,StackRuntime,Settings runtime
    class LocalWorkspace,Stack,ProjectSettings automation
    class DynamicProvider,DynamicResource dynamic
    class Logging,AssetArchive utils
    class User,PulumiCLI,ResourceProviders external
```