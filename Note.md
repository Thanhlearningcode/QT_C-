
### Three GitHub Repositories Meeting the Requirements
- **[gpospelov/qt-mvvm](https://github.com/gpospelov/qt-mvvm)**: Appears to be the best choice with a framework MVVM for large Qt C++ applications.
- **[Skycoder42/QtMvvm](https://github.com/Skycoder42/QtMvvm)**: Suitable for creating multiple interfaces from a core library.
- **[pvanhoof/mvvm](https://github.com/pvanhoof/mvvm)**: Can be used for learning and demonstrating basic MVVM techniques, but lacks detailed information.

### Overview
Based on the requirements, three GitHub repositories using C++, Qt, and applying the MVVM pattern have been identified, suitable for replacing the current "facelog box" application. These repositories appear to be scalable, maintainable, and high-performing, with capabilities to handle memory leaks and recovery mechanisms when the application gets stuck. In particular, they can support state machine management, helping to avoid the issue of the application freezing when it fails to return to the initial state.

### Details of Each Repository
- **[gpospelov/qt-mvvm](https://github.com/gpospelov/qt-mvvm)**:
  - Framework MVVM with features like JSON serialization, undo/redo, and state management.
  - Suitable for large applications, easy to demo adding a new screen.
- **[Skycoder42/QtMvvm](https://github.com/Skycoder42/QtMvvm)**:
  - MVVM library supporting both Widgets and Qt Quick, with Dependency Injection.
  - Scalable and maintainable, with clear illustrative examples.
- **[pvanhoof/mvvm](https://github.com/pvanhoof/mvvm)**:
  - Focuses on MVVM techniques with Qt and QtQuick, with examples of commands and ViewModel.
  - Limited information on performance and recovery.

## Detailed Survey Notes

Based on the requirement to find three GitHub repositories using C++, Qt, applying the MVVM pattern, scalable, maintainable, high-performing, free of memory leaks, and with recovery mechanisms when the application gets stuck, particularly related to the issue of the state machine failing to return to the initial state, an analysis and selection were conducted based on specific criteria. Below is a detailed report on the search and evaluation process.

### Context and Requirements
The requirements include finding repositories to replace the current "facelog box" application, with the following criteria:
- Use C++ and Qt, apply MVVM for the UI.
- Scalable, maintainable, high-performing (no memory leaks), with recovery mechanisms when the application gets stuck.
- The current application has an issue where the state machine fails to return to the initial state, requiring a box reboot.

### Search and Analysis Methodology
Search tools were used with keywords: "Qt C++ MVVM", "high performance", "no memory leaks", "recovery mechanism", "state machine". Repositories were evaluated based on description, features, examples, and ability to meet the criteria.

#### Repository Summary Table
| Repository Name          | Description                                                                 | Language | Stars | Forks | URL                                      |
|--------------------------|----------------------------------------------------------------------|----------|-------|-------|------------------------------------------|
| gpospelov/qt-mvvm        | Framework MVVM for large Qt C++ applications, with serialization and undo/redo | C++      | -     | -     | [gpospelov/qt-mvvm](https://github.com/gpospelov/qt-mvvm) |
| Skycoder42/QtMvvm        | MVVM library for Widgets and Quick, supporting Dependency Injection   | C++      | -     | -     | [Skycoder42/QtMvvm](https://github.com/Skycoder42/QtMvvm) |
| pvanhoof/mvvm            | MVVM techniques with Qt and QtQuick, with command and ViewModel examples | C++      | -     | -     | [pvanhoof/mvvm](https://github.com/pvanhoof/mvvm) |
| Qt-MVC-MVVM (organization) | Contains multiple MVVM-related repositories, but lacks detailed information | C++      | -     | -     | [Qt-MVC-MVVM](https://github.com/Qt-MVC-MVVM) |

### Detailed Evaluation of Each Repository

#### 1. [gpospelov/qt-mvvm](https://github.com/gpospelov/qt-mvvm)
- **Features**:
  - Application model stores GUI session data, supports JSON serialization.
  - Undo/redo based on command pattern, suitable for state management and recovery.
  - ViewModel acts as a proxy for SessionModel, ensuring separation of logic and UI.
  - Example "collidingmice" illustrates data display in QGraphicsScene and QTreeView, with state rollback via slider.
- **Performance and memory management**: Designed for large applications, uses QVariant (to transition to std::variant), ViewModel based on QAbstractItemModel, reducing memory leak risks.
- **Recovery mechanism**: JSON serialization and undo/redo can restore state when the application is stuck.
- **State machine**: No direct mention of QStateMachine, but undo/redo aids state management; QStateMachine can be integrated.
- **Strengths**: Suitable for large applications, detailed examples, easy to demo new screens and present architecture.

#### 2. [Skycoder42/QtMvvm](https://github.com/Skycoder42/QtMvvm)
- **Features**:
  - Clear separation of UI and logic, supports core library with application logic and ViewModels, usable for Widgets and Qt Quick.
  - Dependency Injection via macros and ServiceRegistry, enhancing flexibility and testability.
  - Includes tools like messageboxes, dialogs, bindings, and generic Presenter Interface.
  - Supports QtMvvmDatasync for QtDataSync integration, managing connections and synchronization.
- **Performance and memory management**: Optimized for complex projects, memory management based on Qt, classes inherit QObject, reducing memory leaks.
- **Recovery mechanism**: Can use ViewModels and serialization for state restoration.
- **State machine**: No mention of QStateMachine, but integration is possible.
- **Strengths**: Flexible, supports multiple interfaces, illustrative examples, easy to demo and present architecture.

#### 3. [pvanhoof/mvvm](https://github.com/pvanhoof/mvvm)
- **Features**:
  - Focuses on MVVM techniques with Qt and QtQuick, supporting AbstractCommand, RelayCommand, and asynchronous commands.
  - Links to blog with detailed implementation articles:
    - [AbstractCommand techniques](http://pvanhoof.be/blog/index.php/2017/08/18/abstractcommand-model-view-viewmodel-techniques)
    - [RelayCommand in Qt](http://pvanhoof.be/blog/index.php/2017/08/24/the-relaycommand-in-qt)
    - [Asynchronous commands](http://pvanhoof.be/blog/index.php/2017/10/23/asynchronous-commands)
- **Performance and memory management**: Lacks specific information, small repository, may not be optimized for large applications. Memory management based on Qt.
- **Recovery mechanism**: Can use command pattern for basic state restoration.
- **State machine**: No mention of QStateMachine, but integration is possible.
- **Strengths**: Suitable for learning and basic demos, specific examples, but less information on performance and recovery.

### Comparison and Selection
- All three repositories use C++, Qt, and MVVM, meeting the requirements.
- **[gpospelov/qt-mvvm](https://github.com/gpospelov/qt-mvvm)** and **[Skycoder42/QtMvvm](https://github.com/Skycoder42/QtMvvm)** stand out with comprehensive features, detailed examples, and state management capabilities.
- **[pvanhoof/mvvm](https://github.com/pvanhoof/mvvm)** is more suited for learning and basic demos but lacks performance and recovery details.
- For state machine issues, no repository directly mentions QStateMachine, but all can integrate it, with [gpospelov/qt-mvvm](https://github.com/gpospelov/qt-mvvm) being stronger for recovery due to undo/redo and serialization.

### Detailed Evaluation of Qt MVVM Repositories

## 1. gpospelov/qt-mvvm
- *Features*:
  - Has an application model to store GUI session data, supports JSON serialization, enabling state saving and loading.
  - Supports undo/redo based on command pattern, suitable for state management and can be used for recovery.
  - ViewModel acts as a proxy for SessionModel, ensuring separation of logic and UI, scalable, and maintainable.
  - Includes examples like "collidingmice", illustrating data display in QGraphicsScene and QTreeView, with the ability to revert to previous states via a slider.
- *Performance and memory management*: No specific performance details, but designed for large applications, likely optimized. Memory management based on Qt, with SessionModel using QVariant (to transition to std::variant), and ViewModel based on QAbstractItemModel, reducing memory leak risks.
- *Recovery mechanism*: JSON serialization and undo/redo can be used to restore state when the application is stuck.
- *State machine*: No direct mention of QStateMachine, but undo/redo aids state management, and QStateMachine can be integrated to address the issue of state not returning to initial.
- *Strengths*: Suitable for large applications, detailed examples, easy to demo adding new screens and present architecture.

## 2. Skycoder42/QtMvvm
- *Features*:
  - Clear separation of UI and logic, allows creating a core library containing application logic and ViewModels, supporting both Widgets and Qt Quick.
  - Supports Dependency Injection via macros and ServiceRegistry, increasing flexibility and testability.
  - Includes tools like messageboxes, dialogs, bindings, and a generic Presenter Interface for custom UI.
  - Supports QtMvvmDatasync for QtDataSync integration, aiding connection and synchronization management.
- *Performance and memory management*: No specific details, but designed for complex projects, likely optimized. Memory management based on Qt, with classes inheriting QObject, reducing memory leak risks.
- *Recovery mechanism*: No direct mention, but ViewModels and serialization can be used to restore state.
- *State machine*: No mention of QStateMachine, but can be integrated to manage state, suitable for addressing stuck state machine issues.
- *Strengths*: Flexible, supports multiple interfaces, illustrative examples, easy to demo and present architecture.

## 3. pvanhoof/mvvm
- *Features*:
  - Focuses on MVVM techniques with Qt and QtQuick, including AbstractCommand, RelayCommand, and asynchronous commands.
  - Links to a blog with detailed implementation articles, such as [abstractcommand-model-view-viewmodel-techniques](http://pvanhoof.be/blog/index.php/2017/08/18/abstractcommand-model-view-viewmodel-techniques), [the-relaycommand-in-qt](http://pvanhoof.be/blog/index.php/2017/08/24/the-relaycommand-in-qt), and [asynchronous-commands](http://pvanhoof.be/blog/index.php/2017/10/23/asynchronous-commands).
- *Performance and memory management*: No specific details, but a small repository, possibly not optimized for large applications. Memory management based on Qt, but no detailed mention of memory leaks.
- *Recovery mechanism*: No mention, but command pattern can be used for basic state restoration.
- *State machine*: No mention of QStateMachine, but integration is possible to manage state.
- *Strengths*: Suitable for learning and basic demos, specific examples, but less information on performance and recovery compared to the other two repositories.

### Conclusion
The three repositories meet the requirements as of 14:31, 12/05/2025, but further checks are needed for recovery mechanisms and state machine management. Prepare demos based on [gpospelov/qt-mvvm](https://github.com/gpospelov/qt-mvvm) or [Skycoder42/QtMvvm](https://github.com/Skycoder42/QtMvvm) for the review at 15:00, 14/05/2025.

### Key Citations
- [Model View ViewModel framework for large Qt C++ applications](https://github.com/gpospelov/qt-mvvm)
- [A mvvm oriented library for Qt, to create Projects for Widgets and Quick in parallel](https://github.com/Skycoder42/QtMvvm)
- [Model View ViewModel techniques with Qt and QtQuick](https://github.com/pvanhoof/mvvm)
- [AbstractCommand techniques](http://pvanhoof.be/blog/index.php/2017/08/18/abstractcommand-model-view-viewmodel-techniques)
- [RelayCommand in Qt](http://pvanhoof.be/blog/index.php/2017/08/24/the-relaycommand-in-qt)
- [Asynchronous commands](http://pvanhoof.be/blog/index.php/2017/10/23/asynchronous-commands)
