# CLAUDE.md - Java Concurrency LiveLessons Repository Guide

**Last Updated:** 2025-11-14
**Repository:** Java_concurrency_livelessons
**Purpose:** Educational codebase demonstrating Java concurrency patterns and Android development

---

## Table of Contents

1. [Repository Overview](#repository-overview)
2. [Project Structure](#project-structure)
3. [Application Descriptions](#application-descriptions)
4. [Key Concurrency Patterns](#key-concurrency-patterns)
5. [Design Patterns Used](#design-patterns-used)
6. [Development Workflows](#development-workflows)
7. [Code Conventions](#code-conventions)
8. [Important File Locations](#important-file-locations)
9. [AI Assistant Guidelines](#ai-assistant-guidelines)

---

## Repository Overview

This repository contains **4 independent Android/Java applications** demonstrating various concurrency patterns, synchronization mechanisms, and design patterns. The codebase is educational in nature, designed to teach concurrent programming concepts through practical examples.

### Key Statistics

- **Total Java Files:** 88
- **Applications:** 4 (ExpressionTreeApp, ImageTaskGangApplication, PingPongApplication, ThreadedDownloads)
- **Build System:** Gradle 5.4.1
- **Android SDK:** Target 29, Min 14-18
- **Build Plugin:** Android Gradle Plugin 3.5.0

### Primary Learning Objectives

1. **Thread synchronization** using multiple primitives (synchronized, Semaphore, ReentrantLock, Condition, BlockingQueue)
2. **Executor framework** for task-based concurrency
3. **Android concurrency patterns** (AsyncTask, Handler/Message, Runnable)
4. **Design patterns** implementation (Strategy, Template Method, Factory, Composite, Visitor, Decorator, etc.)
5. **Barrier synchronization** with CountDownLatch
6. **Async completion** with ExecutorCompletionService and Futures

---

## Project Structure

```
Java_concurrency_livelessons/
├── .git/                           # Git repository
├── .gitignore                      # Ignore patterns (build/, .gradle/, *.class, etc.)
├── README.md                       # Minimal project readme
├── CLAUDE.md                       # This file - AI assistant guide
│
├── ExpressionTreeApp/              # Application 1: Expression Tree with Design Patterns
│   ├── app/
│   │   ├── src/main/java/com/example/expressiontree/  (47 Java files)
│   │   ├── src/main/res/          # Android layouts and resources
│   │   ├── src/main/AndroidManifest.xml
│   │   └── build.gradle           # App-level Gradle configuration
│   ├── build.gradle               # Project-level Gradle
│   ├── settings.gradle            # Gradle settings
│   ├── gradle/wrapper/            # Gradle wrapper files
│   ├── gradlew                    # Unix Gradle wrapper script
│   └── gradlew.bat                # Windows Gradle wrapper script
│
├── ImageTaskGangApplication/      # Application 2: Concurrent Image Processing
│   ├── app/
│   │   ├── src/main/java/example/imagetaskgang/  (19 Java files)
│   │   ├── src/main/res/          # Android layouts and resources
│   │   ├── src/main/AndroidManifest.xml
│   │   └── build.gradle
│   ├── build.gradle
│   ├── settings.gradle
│   ├── gradle/wrapper/
│   ├── gradlew
│   └── gradlew.bat
│
├── PingPongApplication/           # Application 3: Synchronization Mechanisms Demo
│   ├── app/
│   │   ├── src/main/java/example/pingpong/  (13 Java files)
│   │   ├── src/main/res/          # Android layouts and resources
│   │   ├── src/main/AndroidManifest.xml
│   │   └── build.gradle
│   ├── build.gradle
│   ├── settings.gradle
│   ├── gradle/wrapper/
│   ├── gradlew
│   └── gradlew.bat
│
└── ThreadedDownloads/             # Application 4: Android Concurrency Strategies
    ├── app/
    │   ├── src/main/java/edu/vuum/mocca/  (8 Java files)
    │   ├── src/main/res/          # Android layouts and resources
    │   ├── src/main/AndroidManifest.xml
    │   └── build.gradle
    ├── build.gradle
    ├── settings.gradle
    ├── gradle/wrapper/
    ├── gradlew
    └── gradlew.bat
```

### Key Observations

- Each application is **completely independent** with its own Gradle build system
- **No shared libraries** or common modules between applications
- Each app has both **CLI** (MainConsole.java) and **Android GUI** (MainActivity.java) entry points (except ThreadedDownloads which is Android-only)
- **No formal unit tests** - applications use manual testing through CLI options and Android UI

---

## Application Descriptions

### 1. ExpressionTreeApp (47 Java files)

**Package:** `com.example.expressiontree`
**Min SDK:** 14 (Android 4.0)
**Purpose:** Demonstrates expression tree processing with extensive design pattern usage

#### Core Functionality

- Parses mathematical expressions into tree structures
- Evaluates expressions using Visitor pattern
- Supports multiple traversal strategies (in-order, pre-order, post-order, level-order)
- Thread-safe tree operations using `synchronized` keyword

#### Key Classes

| Class | Pattern/Role | Location |
|-------|--------------|----------|
| `ExpressionTree` | Bridge pattern abstraction | Bridge to ComponentNode implementation |
| `SynchronizedExpressionTree` | Decorator | Synchronized wrapper using `synchronized` |
| `ComponentNode` | Composite pattern | Abstract base for tree nodes |
| `LeafNode` | Composite pattern | Terminal nodes (operands) |
| `CompositeUnaryNode` | Composite pattern | Unary operator nodes (negate) |
| `CompositeBinaryNode` | Composite pattern | Binary operator nodes (+, -, *, /) |
| `Visitor` | Visitor pattern | Interface for tree visitors |
| `EvaluationVisitor` | Visitor pattern | Evaluates expressions |
| `PrintVisitor` | Visitor pattern | Prints tree structure |
| `UserCommand` | Command pattern | Abstract command interface |
| `InputDispatcher` | Singleton | Event loop for command processing |
| `Platform` | Strategy/Singleton | Platform abstraction (Console/Android) |

#### Concurrency Features

- **Thread Safety:** `synchronized` methods on `SynchronizedExpressionTree`
- **Model:** Blocking/critical sections for shared tree access
- **Use Case:** Protecting shared data structures from concurrent modification

#### Entry Points

- **CLI:** `MainConsole.java` - Command-line interface
- **Android:** `MainActivity.java` - Android GUI with two activity modes (Verbose/Succinct)

---

### 2. ImageTaskGangApplication (19 Java files)

**Package:** `example.imagetaskgang`
**Min SDK:** 18 (Android 4.3)
**Purpose:** High-throughput concurrent image downloading, filtering, and processing

#### Core Functionality

- Downloads images from URLs concurrently
- Applies filters (grayscale, null filter) using Decorator pattern
- Manages task execution with Executor framework
- Demonstrates Proactor pattern with completion handlers

#### Key Classes

| Class | Pattern/Role | Location |
|-------|--------------|----------|
| `TaskGang` | Template Method | Abstract framework for task execution |
| `ImageTaskGang` | Concrete implementation | ExecutorCompletionService-based processing |
| `Filter` | Template Method | Abstract filter base |
| `FilterDecorator` | Decorator pattern | Abstract filter decorator |
| `OutputFilterDecorator` | Decorator pattern | Writes filtered images |
| `GrayScaleFilter` | Concrete filter | Grayscale conversion |
| `ImageEntity` | Model | Image representation |
| `PlatformStrategy` | Strategy pattern | Platform abstraction |

#### Concurrency Features

- **Executor Framework:** `Executors.newCachedThreadPool()`
- **Task Management:** `ExecutorCompletionService<ImageEntity>` for async completion
- **Synchronization:** `CountDownLatch` for barrier synchronization between cycles
- **Tasks:** `Callable<ImageEntity>` for return-value tasks
- **Results:** `Future<ImageEntity>` with exception handling
- **Atomic Operations:** `AtomicLong` for cycle tracking

#### Architecture Pattern

**Proactor Pattern Implementation:**
1. Asynchronous operations initiated via `ExecutorService`
2. Completion handlers process results as they become available
3. `ExecutorCompletionService` manages completion queue
4. Main thread polls for completed tasks

#### Entry Points

- **CLI:** `MainConsole.java` - Console-based batch processing
- **Android:** `MainActivity.java` - Interactive image processing UI

---

### 3. PingPongApplication (13 Java files)

**Package:** `example.pingpong`
**Min SDK:** 14 (Android 4.0)
**Purpose:** Educational demonstration of 4 different thread synchronization mechanisms

#### Core Functionality

- Implements classic ping-pong alternating pattern between two threads
- Demonstrates **4 different synchronization primitives** for the same algorithm
- Uses Template Method pattern to define algorithm structure
- Uses Strategy pattern to plug in different synchronization mechanisms

#### Key Classes

| Class | Pattern/Role | Synchronization Mechanism |
|-------|--------------|---------------------------|
| `PingPongThread` | Template Method | Abstract base with `acquire()`/`release()` hooks |
| `PingPongThreadMonObj` | Concrete implementation | Binary semaphore using monitor objects (`synchronized`, `wait()`, `notify()`) |
| `PingPongThreadSema` | Concrete implementation | `java.util.concurrent.Semaphore` |
| `PingPongThreadCond` | Concrete implementation | `ReentrantLock` + `Condition` variables |
| `PingPongThreadBlockingQueue` | Concrete implementation | `LinkedBlockingQueue<Object>` |
| `PlayPingPong` | Factory/Orchestrator | Creates appropriate synchronization strategy |
| `Options` | Singleton | Command-line option parsing |

#### Synchronization Mechanisms Comparison

| Mechanism | Primitives Used | Complexity | Use Case |
|-----------|-----------------|------------|----------|
| **MONOBJ** | `synchronized`, `wait()`, `notify()` | Medium | Traditional Java synchronization |
| **SEMA** | `java.util.concurrent.Semaphore` | Low | Simple counting/binary semaphore |
| **COND** | `ReentrantLock`, `Condition` | High | Fine-grained control, complex conditions |
| **QUEUE** | `LinkedBlockingQueue<Object>` | Low | Message passing, producer-consumer |

#### Concurrency Features

- **Thread Extension:** Extends `Thread` class directly
- **Template Method:** Algorithm in `run()`, hooks for synchronization
- **Turn-based Execution:** Threads alternate using chosen primitive
- **Barrier Sync:** `CountDownLatch` for completion coordination

#### CLI Options

```bash
Usage: -i <iterations> -s <sync-mechanism>
  -i: max iterations (default: 6)
  -s: SEMA | MONOBJ | COND | QUEUE (default: SEMA)
```

#### Entry Points

- **CLI:** `MainConsole.java` - Command-line ping-pong demonstration
- **Android:** `MainActivity.java` - Android GUI with visual ping-pong

---

### 4. ThreadedDownloads (8 Java files)

**Package:** `edu.vuum.mocca`
**Min SDK:** 14 (Android 4.0)
**Purpose:** Demonstrates 3 different Android concurrency strategies for background operations

#### Core Functionality

- Downloads images from URLs in background threads
- Updates UI with downloaded images
- Demonstrates **3 different concurrency approaches** for the same task
- Shows proper UI thread marshaling and memory management

#### Key Classes

| Class | Pattern/Role | Concurrency Strategy |
|-------|--------------|----------------------|
| `ButtonStrategy` | Strategy interface | Abstract download strategy |
| `DownloadWithRunnable` | Concrete strategy | `Thread` + `Runnable` + `runOnUiThread()` |
| `DownloadWithMessages` | Concrete strategy | `Thread` + `Handler`/`Message` framework (HaMeR) |
| `DownloadWithAsyncTask` | Concrete strategy | `AsyncTask` with lifecycle hooks |
| `DownloadContext` | Context/Model | Manages download state and UI components |
| `ButtonStrategyMapper` | Factory | Maps button IDs to strategies |
| `ThreadedDownloadsActivity` | Controller | Main Android activity |

#### Android Concurrency Strategies Comparison

| Strategy | Complexity | UI Thread Safety | Best For |
|----------|------------|------------------|----------|
| **Runnable** | Low | Manual (`runOnUiThread()`) | Simple background tasks |
| **Handler/Message** | Medium | `Handler.handleMessage()` | Producer-consumer, complex messaging |
| **AsyncTask** | Low | Automatic lifecycle hooks | Quick async operations with UI updates |

#### Concurrency Features

- **Thread Creation:** `new Thread(Runnable).start()`
- **UI Marshaling:** `Activity.runOnUiThread()`, `Handler.post()`, `AsyncTask` hooks
- **Message Passing:** `Handler`/`Message` framework with message types
- **Memory Management:** `WeakReference<Activity>`, `WeakReference<ImageView>` to prevent leaks
- **Lifecycle:** `AsyncTask` hooks: `onPreExecute()`, `doInBackground()`, `onPostExecute()`

#### Android Components

- **Layout:** `main.xml` - Buttons for each strategy + ImageView
- **Manifest:** `AndroidManifest.xml` - Declares activity and permissions

#### Entry Points

- **Android Only:** `ThreadedDownloadsActivity.java` - Interactive download demo

---

## Key Concurrency Patterns

### Concurrency Primitives by Application

| Primitive/Pattern | ExpressionTree | ImageTaskGang | PingPong | ThreadedDownloads |
|-------------------|----------------|---------------|----------|-------------------|
| `Thread` | No | Implicit in Executor | Yes (extends) | Yes (Runnable) |
| `Runnable` | No | Yes (via Callable) | No | Yes |
| `Executor`/`ExecutorService` | No | Yes | No | Implicit (AsyncTask) |
| `Callable<V>` | No | Yes | No | No |
| `Future<V>` | No | Yes | No | No |
| `ExecutorCompletionService` | No | Yes | No | No |
| `CountDownLatch` | No | Yes | Yes | No |
| `Semaphore` | No | No | Yes | No |
| `ReentrantLock` | No | No | Yes | No |
| `Condition` | No | No | Yes | No |
| `LinkedBlockingQueue` | No | No | Yes | No |
| `AtomicLong` | No | Yes | No | No |
| `synchronized` keyword | Yes | No | Yes (MONOBJ) | No |
| `wait()`/`notify()` | No | No | Yes (MONOBJ) | No |
| Android `AsyncTask` | No | No | No | Yes |
| Android `Handler`/`Message` | No | No | No | Yes |

### Concurrency Pattern Categories

#### 1. Low-Level Synchronization

**Files:**
- `ExpressionTreeApp/.../SynchronizedExpressionTree.java` - synchronized methods
- `PingPongApplication/.../PingPongThreadMonObj.java` - wait/notify
- `PingPongApplication/.../PingPongThreadSema.java` - Semaphore
- `PingPongApplication/.../PingPongThreadCond.java` - ReentrantLock + Condition

**When to use:** Direct thread coordination, mutual exclusion, turn-based algorithms

#### 2. High-Level Executor Framework

**Files:**
- `ImageTaskGangApplication/.../TaskGang.java` - Executor-based framework
- `ImageTaskGangApplication/.../ImageTaskGang.java` - ExecutorCompletionService

**When to use:** Task-based parallelism, thread pool management, async completion handling

#### 3. Barrier Synchronization

**Files:**
- `ImageTaskGangApplication/.../ImageTaskGang.java` - CountDownLatch for cycles
- `PingPongApplication/.../PlayPingPong.java` - CountDownLatch for completion

**When to use:** Coordinating multiple threads, waiting for task completion

#### 4. Message Passing

**Files:**
- `PingPongApplication/.../PingPongThreadBlockingQueue.java` - LinkedBlockingQueue
- `ThreadedDownloads/.../DownloadWithMessages.java` - Handler/Message

**When to use:** Producer-consumer patterns, decoupled communication

#### 5. Android Concurrency

**Files:**
- `ThreadedDownloads/.../DownloadWithRunnable.java` - Thread + runOnUiThread
- `ThreadedDownloads/.../DownloadWithMessages.java` - Handler/Message
- `ThreadedDownloads/.../DownloadWithAsyncTask.java` - AsyncTask

**When to use:** Android background operations with UI updates

---

## Design Patterns Used

### Comprehensive Pattern Matrix

| Pattern | Applications | Key Classes | Purpose |
|---------|--------------|-------------|---------|
| **Strategy** | All 4 | `PlatformStrategy`, `ButtonStrategy`, `PingPongThread` | Pluggable algorithms |
| **Template Method** | ExpressionTree, ImageTaskGang, PingPong | `TaskGang`, `PingPongThread`, `Filter` | Algorithm skeleton with hooks |
| **Factory Method** | All 4 | `PlatformFactory`, `UserCommandFactory`, `IteratorFactory` | Object creation |
| **Singleton** | All 4 | `Platform`, `InputDispatcher`, `Options`, `PlatformStrategy` | Single instance |
| **Composite** | ExpressionTree | `ComponentNode`, `LeafNode`, `CompositeBinaryNode` | Tree structures |
| **Visitor** | ExpressionTree | `Visitor`, `EvaluationVisitor`, `PrintVisitor` | Operations on composite |
| **Iterator** | ExpressionTree | `InOrderIterator`, `PreOrderIterator`, `PostOrderIterator` | Collection traversal |
| **Command** | ExpressionTree | `UserCommand`, `EvalCommand`, `PrintCommand`, `MacroCommand` | Encapsulate requests |
| **Decorator** | ExpressionTree, ImageTaskGang | `FilterDecorator`, `SynchronizedExpressionTree` | Add responsibilities |
| **Bridge** | ExpressionTree | `ExpressionTree` (abstraction) + `ComponentNode` (implementor) | Separate abstraction/implementation |
| **Proactor** | ImageTaskGang | `ImageTaskGang` with `ExecutorCompletionService` | Async completion handlers |

### Pattern Implementation Files

#### Composite Pattern
- `ExpressionTreeApp/.../ComponentNode.java` - Component interface
- `ExpressionTreeApp/.../LeafNode.java` - Leaf
- `ExpressionTreeApp/.../CompositeUnaryNode.java` - Composite
- `ExpressionTreeApp/.../CompositeBinaryNode.java` - Composite
- Child classes: `CompositeAddNode`, `CompositeSubtractNode`, `CompositeMultiplyNode`, `CompositeDivideNode`, `CompositeNegateNode`

#### Visitor Pattern
- `ExpressionTreeApp/.../Visitor.java` - Visitor interface
- `ExpressionTreeApp/.../EvaluationVisitor.java` - Concrete visitor (evaluation)
- `ExpressionTreeApp/.../PrintVisitor.java` - Concrete visitor (printing)

#### Iterator Pattern
- `ExpressionTreeApp/.../IteratorFactory.java` - Factory
- `ExpressionTreeApp/.../InOrderIterator.java` - In-order traversal
- `ExpressionTreeApp/.../PreOrderIterator.java` - Pre-order traversal
- `ExpressionTreeApp/.../PostOrderIterator.java` - Post-order traversal
- `ExpressionTreeApp/.../LevelOrderIterator.java` - Level-order traversal

#### Command Pattern
- `ExpressionTreeApp/.../UserCommand.java` - Command interface
- Concrete commands: `EvalCommand`, `PrintCommand`, `SetCommand`, `FormatCommand`, `QuitCommand`, `NullCommand`
- `ExpressionTreeApp/.../MacroCommand.java` - Composite command
- `ExpressionTreeApp/.../UserCommandFactory.java` - Factory

#### Strategy Pattern
- All apps: `Platform`/`PlatformStrategy` - Platform abstraction
- ThreadedDownloads: `ButtonStrategy` - Download strategy
- PingPongApplication: `PingPongThread` implementations - Sync strategies

#### Template Method Pattern
- `ImageTaskGangApplication/.../TaskGang.java` - `run()` template with hooks
- `PingPongApplication/.../PingPongThread.java` - `run()` template with `acquire()`/`release()` hooks
- `ImageTaskGangApplication/.../Filter.java` - Filter template

#### Decorator Pattern
- `ExpressionTreeApp/.../SynchronizedExpressionTree.java` - Adds synchronization
- `ExpressionTreeApp/.../InstrumentedExpressionTree.java` - Adds logging
- `ImageTaskGangApplication/.../FilterDecorator.java` - Abstract filter decorator
- `ImageTaskGangApplication/.../OutputFilterDecorator.java` - Adds file output

---

## Development Workflows

### Building Applications

Each application is built independently using Gradle wrapper.

#### Build Single Application

```bash
# Navigate to application directory
cd ExpressionTreeApp/          # or ImageTaskGangApplication/ or PingPongApplication/ or ThreadedDownloads/

# Build using Gradle wrapper
./gradlew build                 # Unix/Linux/Mac
gradlew.bat build               # Windows

# Clean build
./gradlew clean build

# Build debug APK
./gradlew assembleDebug

# Build release APK
./gradlew assembleRelease
```

#### Build All Applications

```bash
# From repository root
for app in ExpressionTreeApp ImageTaskGangApplication PingPongApplication ThreadedDownloads; do
  cd "$app"
  ./gradlew build
  cd ..
done
```

### Running Applications

#### Console/CLI Execution

Applications with CLI support (ExpressionTreeApp, ImageTaskGangApplication, PingPongApplication):

```bash
# Navigate to application directory
cd PingPongApplication/

# Build first
./gradlew build

# Run main class (example - adjust package and class)
java -cp app/build/classes/java/main example.pingpong.MainConsole

# With options (PingPongApplication example)
java -cp app/build/classes/java/main example.pingpong.MainConsole -i 10 -s COND
```

#### Android Execution

```bash
# Install on connected device/emulator
./gradlew installDebug

# Run installed app via adb
adb shell am start -n <package.name>/.MainActivity

# Examples:
adb shell am start -n com.example.expressiontree/.MainActivity
adb shell am start -n example.imagetaskgang/.MainActivity
adb shell am start -n example.pingpong/.MainActivity
adb shell am start -n edu.vuum.mocca/.ThreadedDownloadsActivity
```

### Testing

**Note:** This repository has **no formal unit tests**. Testing is manual through:

1. **CLI testing** - Run console applications with various options
2. **Android manual testing** - Use Android UI to test functionality
3. **Visual verification** - Check output correctness

#### Manual Testing Checklist

**ExpressionTreeApp:**
- Test expression parsing: `1 + 2 * 3`
- Test evaluation correctness
- Test different traversal orders (in-order, pre-order, post-order, level-order)
- Test format changes
- Test verbose vs. succinct mode

**ImageTaskGangApplication:**
- Test concurrent image downloads
- Test filter application (grayscale, null)
- Test with different URL lists
- Verify all images processed

**PingPongApplication:**
- Test each sync mechanism: `-s SEMA`, `-s MONOBJ`, `-s COND`, `-s QUEUE`
- Test different iteration counts: `-i 10`, `-i 20`
- Verify alternating "Ping" and "Pong" output
- Check all threads complete

**ThreadedDownloads:**
- Test each download strategy button
- Verify image displays correctly
- Test with valid/invalid URLs
- Test cancellation

### Gradle Tasks Reference

```bash
# List all available tasks
./gradlew tasks

# Common tasks
./gradlew clean              # Clean build artifacts
./gradlew build              # Build project
./gradlew assembleDebug      # Build debug APK
./gradlew assembleRelease    # Build release APK
./gradlew installDebug       # Install debug APK on device
./gradlew uninstallAll       # Uninstall app from device
./gradlew dependencies       # Show dependency tree
```

---

## Code Conventions

### Package Naming

- **ExpressionTreeApp:** `com.example.expressiontree`
- **ImageTaskGangApplication:** `example.imagetaskgang`
- **PingPongApplication:** `example.pingpong`
- **ThreadedDownloads:** `edu.vuum.mocca`

### Class Naming Conventions

| Convention | Example | Usage |
|------------|---------|-------|
| Interface suffix | `Visitor.java`, `Image.java` | Interfaces typically noun-based |
| Abstract prefix | None used | Abstract classes have descriptive names |
| Concrete descriptive | `EvaluationVisitor`, `PrintVisitor` | Describes specific implementation |
| Platform suffix | `AndroidPlatform`, `CommandLinePlatform` | Platform-specific implementations |
| Pattern suffix | `UserCommandFactory`, `FilterDecorator` | Pattern role in name |
| Strategy suffix | `ButtonStrategy`, `PlatformStrategy` | Strategy pattern interface |

### Documentation Style

All classes use **JavaDoc-style comments**:

```java
/**
 * @class ClassName
 *
 * @brief Brief one-line description of class purpose
 *
 * @description Detailed multi-line description explaining:
 *              - Design patterns used (e.g., "Implements Visitor pattern")
 *              - Role in architecture (e.g., "Concrete Strategy")
 *              - Concurrency considerations
 *              - Usage examples
 */
public class ClassName {
    /**
     * @brief Brief method description
     *
     * @param paramName Parameter description
     * @return Return value description
     */
    public ReturnType methodName(ParamType paramName) {
        // Implementation
    }
}
```

### Code Style

- **Indentation:** 4 spaces (no tabs)
- **Brace style:** Opening brace on same line
- **Access modifiers:** Explicit on all fields and methods
- **Field prefix:** `m` prefix for member variables (e.g., `mIterator`, `mTree`, `mPlatform`)
- **Constants:** ALL_CAPS with underscores
- **Method names:** camelCase starting with verb

### Concurrency Conventions

#### Synchronization Patterns

**Using synchronized:**
```java
public synchronized void method() {
    // Critical section
}

synchronized (lock) {
    // Critical section
}
```

**Using ReentrantLock:**
```java
private final ReentrantLock mLock = new ReentrantLock();

public void method() {
    mLock.lock();
    try {
        // Critical section
    } finally {
        mLock.unlock();  // Always unlock in finally
    }
}
```

**Using CountDownLatch:**
```java
private CountDownLatch mLatch = new CountDownLatch(count);

// Worker thread
mLatch.countDown();  // Signal completion

// Main thread
mLatch.await();  // Wait for all workers
```

**Using ExecutorService:**
```java
ExecutorService executor = Executors.newCachedThreadPool();
executor.execute(() -> {
    // Task implementation
});
executor.shutdown();
executor.awaitTermination(timeout, TimeUnit.SECONDS);
```

### Android Conventions

**UI Thread Safety:**
```java
// Option 1: runOnUiThread
activity.runOnUiThread(() -> {
    // UI updates
});

// Option 2: Handler
handler.post(() -> {
    // UI updates
});

// Option 3: AsyncTask hooks (automatic)
@Override
protected void onPostExecute(Result result) {
    // Runs on UI thread automatically
}
```

**Memory Management:**
```java
// Use WeakReference for Activity references in background threads
private final WeakReference<Activity> mActivityRef;

public MyClass(Activity activity) {
    mActivityRef = new WeakReference<>(activity);
}

public void doWork() {
    Activity activity = mActivityRef.get();
    if (activity != null && !activity.isFinishing()) {
        // Use activity
    }
}
```

---

## Important File Locations

### Entry Points by Application

| Application | CLI Entry Point | Android Entry Point |
|-------------|-----------------|---------------------|
| ExpressionTreeApp | `app/src/main/java/com/example/expressiontree/MainConsole.java` | `app/src/main/java/com/example/expressiontree/MainActivity.java` |
| ImageTaskGangApplication | `app/src/main/java/example/imagetaskgang/MainConsole.java` | `app/src/main/java/example/imagetaskgang/MainActivity.java` |
| PingPongApplication | `app/src/main/java/example/pingpong/MainConsole.java` | `app/src/main/java/example/pingpong/MainActivity.java` |
| ThreadedDownloads | N/A | `app/src/main/java/edu/vuum/mocca/ThreadedDownloadsActivity.java` |

### Core Framework Files

**ExpressionTreeApp:**
- Bridge pattern: `app/src/main/java/com/example/expressiontree/ExpressionTree.java`
- Composite pattern: `app/src/main/java/com/example/expressiontree/ComponentNode.java`
- Visitor pattern: `app/src/main/java/com/example/expressiontree/Visitor.java`
- Command pattern: `app/src/main/java/com/example/expressiontree/UserCommand.java`
- Synchronization: `app/src/main/java/com/example/expressiontree/SynchronizedExpressionTree.java`

**ImageTaskGangApplication:**
- Task framework: `app/src/main/java/example/imagetaskgang/TaskGang.java`
- Executor-based: `app/src/main/java/example/imagetaskgang/ImageTaskGang.java`
- Filter system: `app/src/main/java/example/imagetaskgang/Filter.java`
- Decorator: `app/src/main/java/example/imagetaskgang/FilterDecorator.java`

**PingPongApplication:**
- Template Method: `app/src/main/java/example/pingpong/PingPongThread.java`
- Monitor objects: `app/src/main/java/example/pingpong/PingPongThreadMonObj.java`
- Semaphore: `app/src/main/java/example/pingpong/PingPongThreadSema.java`
- ReentrantLock: `app/src/main/java/example/pingpong/PingPongThreadCond.java`
- BlockingQueue: `app/src/main/java/example/pingpong/PingPongThreadBlockingQueue.java`
- Orchestrator: `app/src/main/java/example/pingpong/PlayPingPong.java`

**ThreadedDownloads:**
- Strategy interface: `app/src/main/java/edu/vuum/mocca/ButtonStrategy.java`
- Runnable strategy: `app/src/main/java/edu/vuum/mocca/DownloadWithRunnable.java`
- Handler/Message: `app/src/main/java/edu/vuum/mocca/DownloadWithMessages.java`
- AsyncTask: `app/src/main/java/edu/vuum/mocca/DownloadWithAsyncTask.java`
- Context: `app/src/main/java/edu/vuum/mocca/DownloadContext.java`

### Configuration Files

**Gradle:**
- Root build: `<App>/build.gradle`
- App build: `<App>/app/build.gradle`
- Settings: `<App>/settings.gradle`
- Wrapper properties: `<App>/gradle/wrapper/gradle-wrapper.properties`

**Android:**
- Manifest: `<App>/app/src/main/AndroidManifest.xml`
- Layouts: `<App>/app/src/main/res/layout/*.xml`
- Strings: `<App>/app/src/main/res/values/strings.xml`
- Styles: `<App>/app/src/main/res/values/styles.xml`

**Git:**
- `.gitignore` - Ignores build/, .gradle/, *.class, IDE files, etc.

---

## AI Assistant Guidelines

### When Working with This Repository

#### Understanding Context

1. **Recognize this is educational code** - Focus is on demonstrating patterns clearly, not production optimization
2. **Each application is independent** - Changes in one app don't affect others
3. **No shared libraries** - Pattern implementations are duplicated across apps for pedagogical clarity
4. **CLI and Android dual nature** - Most apps support both console and Android execution

#### Making Code Changes

##### DO:

- **Preserve existing design patterns** - Don't refactor away patterns; they're intentional
- **Maintain concurrency primitives** - If code uses `synchronized`, don't change to `ReentrantLock` without good reason
- **Follow existing conventions** - Use `m` prefix for fields, JavaDoc style, etc.
- **Test both CLI and Android** - If modifying shared code, verify both entry points work
- **Document pattern usage** - Add JavaDoc comments explaining design patterns used
- **Keep applications independent** - Don't create shared modules without explicit request
- **Preserve educational clarity** - Code should be readable and demonstrative

##### DON'T:

- **Don't "optimize away" patterns** - e.g., removing Factory if it seems "unnecessary"
- **Don't add production concerns** - No need for logging frameworks, monitoring, etc. unless requested
- **Don't add unit tests** unless explicitly requested - This repo uses manual testing
- **Don't consolidate apps** - They're meant to be separate examples
- **Don't add complex dependencies** - Keep dependencies minimal
- **Don't modernize concurrency** - Keep old-style patterns (synchronized, wait/notify) for educational value

#### Common Tasks

##### Adding a New Synchronization Mechanism

If asked to add a new sync mechanism to PingPongApplication:

1. Create new class extending `PingPongThread`
2. Implement `acquire()` and `release()` hooks with new primitive
3. Add factory case in `PlayPingPong.makePingPongThreads()`
4. Update `Options` to support new flag
5. Document the new mechanism in JavaDoc

##### Adding a New Filter to ImageTaskGangApplication

1. Create new class extending `Filter`
2. Implement `filter(Image)` method with transformation logic
3. Optionally create decorator extending `FilterDecorator`
4. Register in filter factory/selection logic
5. Document filter algorithm

##### Adding a New Visitor to ExpressionTreeApp

1. Create class implementing `Visitor` interface
2. Implement all visit methods: `visit(LeafNode)`, `visit(CompositeNegateNode)`, etc.
3. Add command to trigger visitor in `UserCommandFactory`
4. Create corresponding `UserCommand` subclass
5. Document visitor's purpose and traversal strategy

##### Debugging Concurrency Issues

**Common issues:**

1. **Deadlock:** Check lock acquisition order, ensure all locks released
2. **Race conditions:** Verify proper synchronization on shared state
3. **UI thread errors:** Ensure Android UI updates on UI thread
4. **Memory leaks:** Check for strong references to Activity in background threads

**Debugging approach:**

```java
// Add logging to track thread execution
System.out.println(Thread.currentThread().getName() + ": " + message);

// Add thread state logging
System.out.println("Thread state: " + thread.getState());

// Log lock acquisition
System.out.println("Acquiring lock...");
lock.lock();
System.out.println("Lock acquired");
```

#### Building and Testing

Before committing changes:

```bash
# Build affected application
cd <Application>/
./gradlew clean build

# For Android apps, install and manually test
./gradlew installDebug

# For CLI apps, run with test inputs
java -cp app/build/classes/java/main <package>.<MainClass> <args>
```

#### File Navigation Tips

**To find concurrency code:**
```bash
# Find all uses of Executor
grep -r "Executor" --include="*.java"

# Find all synchronized methods
grep -r "synchronized" --include="*.java"

# Find all CountDownLatch usage
grep -r "CountDownLatch" --include="*.java"
```

**To find pattern implementations:**
```bash
# Find all Visitor implementations
grep -r "implements Visitor" --include="*.java"

# Find all Factory classes
find . -name "*Factory.java"

# Find all Strategy implementations
grep -r "implements.*Strategy" --include="*.java"
```

#### Common Questions and Answers

**Q: Why are there duplicate Platform/PlatformStrategy classes across apps?**
A: Educational clarity. Each app is self-contained for teaching purposes.

**Q: Why no modern Java features (lambdas, streams, etc.)?**
A: Code targets older Android SDK versions (14-18) and emphasizes traditional patterns.

**Q: Why no reactive programming (RxJava, Project Reactor)?**
A: Focus is on fundamental Java concurrency primitives, not frameworks.

**Q: Should I add unit tests?**
A: Only if explicitly requested. This is educational code tested manually.

**Q: Can I refactor to use modern concurrency libraries?**
A: Only if requested. Default is to preserve educational patterns.

**Q: Why are apps Android-based?**
A: Original use case was Android concurrency education, but CLI versions exist for most apps.

### Response Templates

When asked about concurrency patterns:

```
The [pattern name] is implemented in [Application]/app/src/main/java/[package]/[ClassName].java.

Key aspects:
- Primitive used: [Semaphore/ReentrantLock/synchronized/etc.]
- Synchronization points: [where threads coordinate]
- Use case: [what problem it solves]

Related files:
- [File1.java:line] - [description]
- [File2.java:line] - [description]

This pattern demonstrates [educational purpose].
```

When asked about design patterns:

```
The [pattern name] is used in [Application] with the following structure:

Pattern Role | Class | File Location
-------------|-------|---------------
[Role 1] | [Class1] | [path:line]
[Role 2] | [Class2] | [path:line]

Key methods:
- [Class].[method()] - [purpose]

This pattern provides [benefit] for [use case].
```

### Modification Checklist

Before making changes, verify:

- [ ] Understand which application(s) are affected
- [ ] Identified all relevant design patterns in the code
- [ ] Checked if both CLI and Android entry points exist
- [ ] Reviewed concurrency primitives used
- [ ] Planned how to maintain educational clarity
- [ ] Considered impact on existing patterns
- [ ] Ready to test both CLI and Android (if applicable)
- [ ] Will update JavaDoc comments

After making changes:

- [ ] Code builds successfully (`./gradlew build`)
- [ ] CLI application runs correctly (if applicable)
- [ ] Android application installs and runs (if applicable)
- [ ] JavaDoc comments updated
- [ ] Design patterns preserved or intentionally modified
- [ ] Concurrency behavior verified
- [ ] Educational value maintained

---

## Quick Reference

### Concurrency Primitive Quick Lookup

| Need | Use | Example File |
|------|-----|--------------|
| Mutual exclusion | `synchronized`, `ReentrantLock` | `SynchronizedExpressionTree.java`, `PingPongThreadCond.java` |
| Thread coordination | `wait()`/`notify()`, `Condition` | `PingPongThreadMonObj.java`, `PingPongThreadCond.java` |
| Counting/Binary semaphore | `Semaphore` | `PingPongThreadSema.java` |
| Barrier synchronization | `CountDownLatch` | `ImageTaskGang.java`, `PlayPingPong.java` |
| Message passing | `LinkedBlockingQueue` | `PingPongThreadBlockingQueue.java` |
| Task parallelism | `ExecutorService` | `TaskGang.java`, `ImageTaskGang.java` |
| Async completion | `ExecutorCompletionService`, `Future` | `ImageTaskGang.java` |
| Atomic operations | `AtomicLong` | `TaskGang.java` |
| Android background work | `AsyncTask`, `Handler` | `DownloadWithAsyncTask.java`, `DownloadWithMessages.java` |

### Design Pattern Quick Lookup

| Pattern | Best Example File |
|---------|-------------------|
| Strategy | `PlatformStrategy.java`, `ButtonStrategy.java` |
| Template Method | `PingPongThread.java`, `TaskGang.java` |
| Factory Method | `UserCommandFactory.java`, `PlatformStrategyFactory.java` |
| Singleton | `Platform.java`, `InputDispatcher.java` |
| Composite | `ComponentNode.java` hierarchy |
| Visitor | `Visitor.java`, `EvaluationVisitor.java` |
| Iterator | `InOrderIterator.java`, `PreOrderIterator.java` |
| Command | `UserCommand.java`, `EvalCommand.java` |
| Decorator | `FilterDecorator.java`, `SynchronizedExpressionTree.java` |
| Bridge | `ExpressionTree.java` + `ComponentNode.java` |
| Proactor | `ImageTaskGang.java` with `ExecutorCompletionService` |

### Build Commands Quick Reference

```bash
# Single app build
cd <App>/ && ./gradlew build

# Clean build
cd <App>/ && ./gradlew clean build

# Install on Android device
cd <App>/ && ./gradlew installDebug

# Run CLI (example for PingPong)
cd PingPongApplication/
java -cp app/build/classes/java/main example.pingpong.MainConsole -i 10 -s COND
```

---

## Version History

- **2025-11-14:** Initial CLAUDE.md created with comprehensive repository analysis
  - 4 applications documented
  - 88 Java files catalogued
  - Concurrency patterns mapped
  - Design patterns identified
  - Build workflows documented
  - AI assistant guidelines established

---

## Additional Resources

### Learning Path Recommendations

1. **Start with PingPongApplication** - Simplest concurrency demo, clear synchronization mechanisms
2. **Move to ThreadedDownloads** - Practical Android patterns
3. **Study ImageTaskGangApplication** - Advanced executor framework usage
4. **Explore ExpressionTreeApp** - Complex design pattern interactions

### External References

- Java Concurrency in Practice (book reference)
- Android Developer Documentation: https://developer.android.com/guide/background
- Java `java.util.concurrent` package documentation
- Design Patterns: Elements of Reusable Object-Oriented Software (Gang of Four)

---

**End of CLAUDE.md** - For AI assistants working with Java Concurrency LiveLessons repository
