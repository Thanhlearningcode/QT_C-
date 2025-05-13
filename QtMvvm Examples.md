# Architecture Analysis of QtMvvm Examples

To represent the architecture of the source code in the `examples` directory of the **QtMvvm** repository (https://github.com/Skycoder42/QtMvvm/tree/master/examples), this document analyzes the directory structure, sample projects, and how they embody the **MVVM (Model-View-ViewModel)** pattern. It describes how components (Model, ViewModel, View, Presenter) are organized and interact in specific examples. Finally, it provides a textual overview of the architecture with a simple diagram.

## 1. Overview of the `examples` Directory

The `examples` directory contains sample projects that demonstrate how to use **QtMvvm** to build applications following the MVVM pattern. These projects are organized into subdirectories, each representing an application or a specific aspect of QtMvvm:

- **`mvvmquick`**: A sample project using **Qt Quick** (QML) for the user interface.
- **`mvvmwidgets`**: A sample project using **Qt Widgets** for traditional desktop interfaces.
- **`datasyncquick`**: A sample project integrating **QtDataSync** with a Qt Quick interface.
- **`datasyncwidgets`**: A sample project integrating QtDataSync with a Qt Widgets interface.

Each sample project follows a similar structure, including:
- **Core Project**: Contains the application logic (Model and ViewModel), written in C++.
- **GUI Project**: Contains the user interface (View), written in QML (for Quick) or C++ (for Widgets).
- Additional components like Presenter or QtDataSync integration (if applicable).

## 2. Architecture Analysis via the `mvvmquick` Project

For clarity, this analysis focuses on the `mvvmquick` project (https://github.com/Skycoder42/QtMvvm/tree/master/examples/mvvmquick), as it represents MVVM implementation with Qt Quick. Other projects (`mvvmwidgets`, `datasyncquick`, `datasyncwidgets`) have similar structures but differ in interface type or additional features.

### 2.1. Directory Structure of `mvvmquick`

```
mvvmquick/
├── SampleCore/              # Core Project (Model and ViewModel)
│   ├── coreapp.cpp          # Main application class
│   ├── coreapp.h
│   ├── sampleviewmodel.cpp  # Sample ViewModel
│   ├── sampleviewmodel.h
│   └── SampleCore.pro       # qmake configuration file
├── SampleQuick/             # GUI Project (View)
│   ├── main.cpp             # Application entry point
│   ├── qml.qrc              # QML resources
│   ├── SampleQuick.pro      # qmake configuration file
│   └── qml/
│       ├── Main.qml         # Main interface
│       ├── SampleView.qml   # Primary View, linked to SampleViewModel
│       └── [...]            # Other QML files
└── mvvmquick.pro            # Overall qmake file
```

- **SampleCore**: The **core project**, containing application logic (Model and ViewModel). This part is independent of the interface and reusable for other GUI projects (Widgets or Quick).
- **SampleQuick**: The **GUI project**, containing the Qt Quick interface (View) written in QML, linked to the ViewModel from SampleCore.
- **mvvmquick.pro**: The overall qmake file, linking the two subprojects (SampleCore and SampleQuick).

### 2.2. Architectural Components

Based on the MVVM pattern, the main components in `mvvmquick` are organized as follows:

1. **Model**:
   - Not explicitly implemented in this sample project (assumed to be simple business logic, e.g., static data or file handling).
   - In practice, the Model would reside in `SampleCore`, potentially as C++ classes handling data operations (e.g., database queries, API calls).

2. **ViewModel**:
   - **Files**: `SampleCore/sampleviewmodel.h` and `sampleviewmodel.cpp`.
   - **Role**: Acts as a bridge between Model and View, providing data and handling user interactions.
   - **Details**:
     - The `SampleViewModel` class inherits from `QtMvvm::ViewModel`.
     - Defines **Q_PROPERTY** attributes to provide data to the View (e.g., `input`, `files`, `color`).
     - Defines **Q_INVOKABLE** methods or **slots** to handle commands from the View (e.g., `getInput()`, `getFiles()`, `showProgress()`).
     - Example code from `sampleviewmodel.h`:
       ```cpp
       Q_PROPERTY(QString input READ input WRITE setInput NOTIFY inputChanged)
       Q_INVOKABLE void getInput();
       Q_INVOKABLE void getFiles();
       ```
     - The ViewModel uses Qt's **signals/slots** mechanism to notify the View of data changes.

3. **View**:
   - **Files**: `SampleQuick/qml/SampleView.qml` and other QML files.
   - **Role**: Displays data from the ViewModel and sends user events (e.g., button clicks) to the ViewModel.
   - **Details**:
     - `SampleView.qml` is the main interface, using **data binding** to link to `SampleViewModel` properties.
     - Uses Qt Quick components like `ApplicationWindow`, `MenuBar`, `TextField`, `Button`.
     - Example code from ` parejas View.qml`:
       ```qml
       ApplicationWindow {
           visible: true
           title: qsTr("Sample App")
           MenuBar {
               Menu {
                   title: qsTr("&Sample")
                   MenuItem {
                       text: qsTr("Another Input")
                       onTriggered: viewModel.getInput()
                   }
                   MenuItem {
                       text: qsTr("Add Files")
                       onTriggered: viewModel.getFiles()
                   }
               }
           }
           TextField {
               text: viewModel.input
               onTextEdited: viewModel.input = text
           }
       }
       ```
     - The View links to the ViewModel via the `viewModel` variable (injected by the QtMvvm framework).
     - Events like `onTriggered` call Q_INVOKABLE methods in the ViewModel (e.g., `viewModel.getFiles()`).

4. **Presenter**:
   - **Role**: Manages how Views are displayed (e.g., in the main window, dialog, or subwindow).
   - **Implementation**: Not explicitly shown in the sample code, but QtMvvm provides `QtMvvm::IPresenter` (in the QtMvvmCore library).
   - **Mechanism**:
     - The Presenter is registered with `ServiceRegistry` in `coreapp.cpp`.
     - In `mvvmquick`, Views like `SampleView.qml` are displayed via `App::show()` (in `main.cpp`), using QtMvvm's default Presenter.
     - Example from `SampleQuick/main.cpp`:
       ```cpp
       #include <QtMvvmCore/App>
       #include <SampleCore/coreapp.h>
       int main(int argc, char *argv[]) {
           QGuiApplication app(argc, argv);
           QtMvvm::App<CoreApp> mvvmApp;
           return mvvmApp.exec();
       }
       ```
     - `QtMvvm::App` automatically initializes the Presenter and displays the main View.

5. **Core Application**:
   - **Files**: `SampleCore/coreapp.h` and `coreapp.cpp`.
   - **Role**: The main application class, initializing services (e.g., Presenter, ServiceRegistry) and managing the application lifecycle.
   - **Details**:
     - Inherits from `QtMvvm::CoreApp`.
     - Registers ViewModels and Presenters.
     - Example from `coreapp.cpp`:
       ```cpp
       CoreApp::CoreApp(QObject *parent) :
           QtMvvm::CoreApp(parent)
       {
           registerViewModel<SampleViewModel>();
       }
       ```
     - Ensures the ViewModel (`SampleViewModel`) is available to the GUI project.

### 2.3. Workflow in `mvvmquick`

1. **Startup**:
   - The application starts from `SampleQuick/main.cpp`, initializing `QtMvvm::App` with `CoreApp`.
   - `CoreApp` registers `SampleViewModel` and services.

2. **View Display**:
   - `QtMvvm::App` uses the Presenter to display `SampleView.qml` in an `ApplicationWindow`.

3. **User Interaction**:
   - The user clicks the "Add Files" menu item in `SampleView.qml`.
   - The event triggers `viewModel.getFiles()` in `SampleViewModel`.
   - The ViewModel opens a `QFileDialog` (or similar logic), retrieves a file list, and updates the `files` property.
   - The View automatically updates via binding (Q_PROPERTY NOTIFY).

4. **Data Updates**:
   - When the user types in the `TextField`, the `viewModel.input` property is updated.
   - The ViewModel emits the `inputChanged` signal, and the View reflects the change instantly.

## 3. Comparison with Other Sample Projects

Other projects in `examples` have similar structures but differ in interface or features:

- **`mvvmwidgets`**:
  - Uses **Qt Widgets** (C++ with classes like `QMainWindow`, `QWidget`) instead of QML.
  - Views are implemented in `.cpp` and `.ui` files (Qt Designer).
  - Connects to the ViewModel via **signals/slots** instead of QML binding.
  - Example: A `QPushButton` might connect to the `getFiles()` slot of the ViewModel.

- **`datasyncquick` and `datasyncwidgets`**:
  - Integrate **QtDataSync** for data synchronization.
  - Add ViewModels/Views to display sync status (e.g., "Connected", "Syncing").
  - Maintain the same structure: Core project (ViewModel + sync logic) and GUI project (Quick or Widgets).

## 4. Overall Architecture Representation

Below is the architectural representation of the sample projects in `examples`, focusing on `mvvmquick`:

### 4.1. Architecture Diagram (Text-based)

```
[User]
     |
     v
[View: SampleView.qml]
     |  (Data Binding: Q_PROPERTY)
     |  (Events: Q_INVOKABLE, signals/slots)
     v
[ViewModel: SampleViewModel]
     |  (Business Logic, Data Processing)
     v
[Model: Data Logic - Assumed]
     |
[Presenter: QtMvvm::IPresenter]
     |  (Manages View Display)
     v
[CoreApp: Initializes Application, Registers Services]
```

### 4.2. Interaction Flow

1. **CoreApp** initializes the application, registering ViewModel and Presenter.
2. **Presenter** displays the **View** (SampleView.qml).
3. **View** binds to the **ViewModel** (SampleViewModel) via Q_PROPERTY.
4. The user interacts with the View (clicks buttons, enters text).
5. The View calls Q_INVOKABLE methods or updates properties in the ViewModel.
6. The ViewModel processes logic, updates data, and notifies the View via signals.
7. The View automatically updates the interface based on ViewModel changes.

### 4.3. Source Code Organization

- **SampleCore** (C++):
  - `coreapp.cpp/h`: Manages the application.
  - `sampleviewmodel.cpp/h`: Handles logic and data for the View.
- **SampleQuick** (QML + C++):
  - `main.cpp`: Entry point, initializes QtMvvm::App.
  - `qml/SampleView.qml`: Main interface, binds to ViewModel.
  - `qml.qrc`: QML resources.

## 5. Architectural Characteristics

- **Clear Separation**:
  - **Core Project** (SampleCore) contains logic, independent of the interface.
  - **GUI Project** (SampleQuick) handles only the interface, reusing logic from Core.
- **Reusability**:
  - SampleCore can be used for both Quick and Widgets (as in `mvvmwidgets`).
- **Flexibility**:
  - Presenter allows customization of View display (dialog, subwindow).
  - QtDataSync integrates seamlessly (as in `datasyncquick`).
- **Qt Mechanisms**:
  - Uses **Q_PROPERTY**, **signals/slots**, and **QML binding** to connect View-ViewModel.
  - Leverages **ServiceRegistry** of QtMvvm for service management.

## 6. Conclusion

The architecture of the source code in the `examples` directory of **QtMvvm** clearly demonstrates the MVVM pattern, with separation between **Model** (data logic), **ViewModel** (data processing and interaction), and **View** (interface). The `mvvmquick` sample project illustrates how to use Qt Quick to build an interface, linking to the ViewModel via QML binding and managing display through the Presenter. Other projects (`mvvmwidgets`, `datasyncquick`, `datasyncwidgets`) extend this concept to Qt Widgets or integrate QtDataSync, while maintaining the core MVVM structure.

For further analysis of specific files (e.g., `SampleView.qml`, `sampleviewmodel.cpp`) or a graphical architecture diagram (described for drawing), please provide additional details.
