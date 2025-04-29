# Mod Organizer 2 Oblivion Plugin Diagrams

This document provides visual diagrams of the classes and their relationships in the Mod Organizer 2 Oblivion plugin.

## Class Hierarchy

The following diagram shows the inheritance hierarchy of the classes in the Oblivion plugin:

```mermaid
classDiagram
    GameGamebryo <|-- GameOblivion
    
    GamebryoBSAInvalidation <|-- OblivionBSAInvalidation
    GamebryoDataArchives <|-- OblivionDataArchives
    GamebryoModDataChecker <|-- OblivionModDataChecker
    GamebryoModDataContent <|-- OblivionModDataContent
    GamebryoSaveGame <|-- OblivionSaveGame
    GamebryoScriptExtender <|-- OblivionScriptExtender
    
    class GameOblivion {
        +init(IOrganizer* moInfo)
        +gameName() const
        +executables() const
        +executableForcedLoads() const
        +initializeProfile(const QDir& path, ProfileSettings settings) const
        +steamAPPId() const
        +primaryPlugins() const
        +gameShortName() const
        +validShortNames() const
        +gameNexusName() const
        +iniFiles() const
        +DLCPlugins() const
        +savegameExtension() const
        +savegameSEExtension() const
        +makeSaveGame(QString filePath) const
    }
    
    class OblivionBSAInvalidation {
        +invalidationBSAName() const
        +bsaVersion() const
    }
    
    class OblivionDataArchives {
        +vanillaArchives() const
        +archives(const MOBase::IProfile* profile) const
        +writeArchiveList(MOBase::IProfile* profile, const QStringList& before)
    }
    
    class OblivionModDataChecker {
        +dataLooksValid(std::shared_ptr<const MOBase::IFileTree> fileTree) const
        +fix(std::shared_ptr<MOBase::IFileTree> fileTree) const
        +possibleFolderNames() const
        +possibleFileExtensions() const
    }
    
    class OblivionModDataContent {
        -m_Enabled[CONTENT_MCM] = false
        -m_Enabled[CONTENT_SKYPROC] = false
    }
    
    class OblivionSaveGame {
        +fetchInformationFields(FileWrapper& wrapper, unsigned long& saveNumber, QString& playerName, unsigned short& playerLevel, QString& playerLocation, SYSTEMTIME& creationTime) const
        +fetchDataFields() const
    }
    
    class OblivionScriptExtender {
        +BinaryName() const
        +PluginPath() const
    }
```

## Feature Registration

The following diagram shows how the game features are registered in the Oblivion plugin:

```mermaid
flowchart TD
    GameOblivion -- registers --> OblivionScriptExtender
    GameOblivion -- registers --> OblivionDataArchives
    GameOblivion -- registers --> OblivionBSAInvalidation
    GameOblivion -- registers --> GamebryoSaveGameInfo
    GameOblivion -- registers --> GamebryoLocalSaveGames
    GameOblivion -- registers --> OblivionModDataChecker
    GameOblivion -- registers --> OblivionModDataContent
    GameOblivion -- registers --> GamebryoGamePlugins
    GameOblivion -- registers --> GamebryoUnmanagedMods
```

## Custom vs. Direct Gamebryo Features

The following diagram shows which features are implemented specifically for Oblivion and which are used directly from the Gamebryo framework:

```mermaid
flowchart TD
    subgraph Custom Implementations
        OblivionBSAInvalidation
        OblivionDataArchives
        OblivionModDataChecker
        OblivionModDataContent
        OblivionSaveGame
        OblivionScriptExtender
    end
    
    subgraph Direct Gamebryo Usage
        GamebryoGamePlugins
        GamebryoLocalSaveGames
        GamebryoSaveGameInfo
        GamebryoUnmanagedMods
    end
    
    GameOblivion --> Custom Implementations
    GameOblivion --> Direct Gamebryo Usage
```

## Oblivion Plugin Architecture

The following diagram shows the overall architecture of the Oblivion plugin:

```mermaid
flowchart TD
    subgraph Mod Organizer 2
        IPluginGame
        IOrganizer
    end
    
    subgraph Gamebryo Framework
        GameGamebryo
        GamebryoBSAInvalidation
        GamebryoDataArchives
        GamebryoModDataChecker
        GamebryoModDataContent
        GamebryoSaveGame
        GamebryoScriptExtender
        GamebryoGamePlugins
        GamebryoLocalSaveGames
        GamebryoSaveGameInfo
        GamebryoUnmanagedMods
    end
    
    subgraph Oblivion Plugin
        GameOblivion
        OblivionBSAInvalidation
        OblivionDataArchives
        OblivionModDataChecker
        OblivionModDataContent
        OblivionSaveGame
        OblivionScriptExtender
    end
    
    IPluginGame <-- implements --> GameGamebryo
    GameGamebryo <-- extends --> GameOblivion
    
    GamebryoBSAInvalidation <-- extends --> OblivionBSAInvalidation
    GamebryoDataArchives <-- extends --> OblivionDataArchives
    GamebryoModDataChecker <-- extends --> OblivionModDataChecker
    GamebryoModDataContent <-- extends --> OblivionModDataContent
    GamebryoSaveGame <-- extends --> OblivionSaveGame
    GamebryoScriptExtender <-- extends --> OblivionScriptExtender
    
    GameOblivion -- uses --> GamebryoGamePlugins
    GameOblivion -- uses --> GamebryoLocalSaveGames
    GameOblivion -- uses --> GamebryoSaveGameInfo
    GameOblivion -- uses --> GamebryoUnmanagedMods
    
    IOrganizer -- provides --> GameOblivion
```

## Initialization Flow

The following diagram shows the initialization flow of the Oblivion plugin:

```mermaid
sequenceDiagram
    participant MO2 as Mod Organizer 2
    participant GO as GameOblivion
    participant OSE as OblivionScriptExtender
    participant ODA as OblivionDataArchives
    participant OBSA as OblivionBSAInvalidation
    participant GSGI as GamebryoSaveGameInfo
    participant GLSG as GamebryoLocalSaveGames
    participant OMDC as OblivionModDataChecker
    participant OMDC2 as OblivionModDataContent
    participant GGP as GamebryoGamePlugins
    participant GUM as GamebryoUnmanagedMods
    
    MO2->>GO: init(IOrganizer* moInfo)
    GO->>GO: GameGamebryo::init(moInfo)
    GO->>ODA: create
    GO->>OSE: create & register
    GO->>ODA: register
    GO->>OBSA: create & register
    GO->>GSGI: create & register
    GO->>GLSG: create & register
    GO->>OMDC: create & register
    GO->>OMDC2: create & register
    GO->>GGP: create & register
    GO->>GUM: create & register
    GO->>MO2: return true
```

These diagrams should help visualize the structure and relationships of the Oblivion plugin for Mod Organizer 2.
