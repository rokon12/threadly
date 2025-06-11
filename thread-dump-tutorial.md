# Java Thread Dump Analysis: A Complete Guide

Thread dumps are essential for diagnosing Java application performance issues, deadlocks, and concurrency problems. This tutorial covers how to capture thread dumps and analyze them using Threadly.

## What is a Thread Dump?

A thread dump is a snapshot of all threads in a Java Virtual Machine (JVM) at a specific moment in time. It shows what each thread is doing, including:

- Thread state (RUNNABLE, WAITING, BLOCKED, etc.)
- Stack traces showing method calls
- Lock information and synchronization details
- Thread priorities and daemon status

## How to Capture Thread Dumps

### 1. Using jstack (Traditional Format)

```bash
# Find your Java process ID
jps -l

# Generate thread dump to file
jstack <pid> > threaddump.txt

# Or output to console
jstack <pid>
```

### 2. Using jstack with JSON Format (Recommended for Threadly)

```bash
# Generate JSON formatted thread dump
jstack -format=json <pid> > threaddump.json
```

### 3. Using jcmd

```bash
# List available Java processes
jcmd

# Generate thread dump
jcmd <pid> Thread.print > threaddump.txt

# Generate with JSON format (Java 21+)
jcmd <pid> Thread.print -format=json > threaddump.json
```

### 4. Using kill command (Linux/macOS)

```bash
# Send QUIT signal to Java process
kill -3 <pid>
```

Note: This outputs the thread dump to the application's standard output or log file.

### 5. Using JVisualVM

1. Open JVisualVM
2. Connect to your Java application
3. Go to "Threads" tab
4. Click "Thread Dump" button
5. Export as needed

### 6. Programmatically from Java Code

```java
ThreadMXBean threadMXBean = ManagementFactory.getThreadMXBean();
ThreadInfo[] threadInfos = threadMXBean.dumpAllThreads(true, true);

// Process threadInfos as needed
for (ThreadInfo threadInfo : threadInfos) {
    System.out.println(threadInfo);
}
```

## When to Take Thread Dumps

- **High CPU usage**: Identify threads consuming excessive CPU
- **Application hanging**: Find deadlocks or threads waiting indefinitely
- **Poor performance**: Analyze thread contention and blocking
- **Memory issues**: Identify threads holding references
- **Periodic monitoring**: Regular health checks

## Analyzing Thread Dumps with Threadly

### Step 1: Generate JSON Thread Dump

Use the recommended jstack JSON format:

```bash
jstack -format=json <pid> > my-threaddump.json
```

### Step 2: Open Threadly

1. Navigate to [Threadly](https://yourdomain.com/threadly)
2. Choose your preferred input method:
   - **File Upload**: Drag and drop your JSON file
   - **Copy & Paste**: Paste JSON content directly

### Step 3: Explore the Analysis

Once loaded, Threadly provides:

#### Thread Tree Visualization
- Hierarchical view of all threads
- Expandable containers for thread groups
- Color-coded thread states
- Search and filter capabilities

#### Performance Insights
- Thread state distribution
- Pie chart visualization
- Performance recommendations
- Resource utilization analysis

#### Deadlock Detection
- Automatic cycle detection in wait graphs
- Visual deadlock warnings
- Detailed deadlock analysis
- Root cause identification

#### Advanced Features
- **Search**: Find specific threads by name or state
- **Filter**: Focus on particular thread types
- **Export**: Save analysis as HTML or text
- **Keyboard Shortcuts**: Quick navigation (Ctrl+F, Ctrl+E, etc.)

## Common Thread States Explained

- **RUNNABLE**: Thread is executing or ready to execute
- **WAITING**: Thread is waiting indefinitely for another thread
- **TIMED_WAITING**: Thread is waiting for a specified period
- **BLOCKED**: Thread is blocked waiting for a monitor lock
- **NEW**: Thread has been created but not started
- **TERMINATED**: Thread has completed execution

## Thread Dump Analysis Best Practices

### 1. Take Multiple Dumps
Capture 3-5 thread dumps with 10-30 second intervals to identify patterns.

### 2. Focus on Problem Areas
- Look for threads in BLOCKED state
- Identify threads with similar stack traces
- Check for deadlock cycles

### 3. Analyze Thread Patterns
- High number of threads in WAITING state might indicate poor resource utilization
- Many BLOCKED threads suggest lock contention
- Repeated stack traces indicate hot spots

### 4. Use Filtering
Threadly's filtering helps focus on:
- Specific thread states
- Thread name patterns
- Particular thread groups

## Troubleshooting Common Issues

### High CPU Usage
1. Look for threads in RUNNABLE state
2. Check stack traces for CPU-intensive operations
3. Identify tight loops or inefficient algorithms

### Application Hanging
1. Search for BLOCKED threads
2. Use deadlock detection feature
3. Analyze wait chains and lock dependencies

### Memory Leaks
1. Examine threads holding references
2. Look for accumulating thread counts
3. Check for threads not being properly cleaned up

## Privacy and Security

Threadly operates entirely client-side:
- No data is sent to external servers
- Analysis happens in your browser
- Thread dumps remain on your machine
- Perfect for sensitive production environments

## Conclusion

Thread dump analysis is crucial for maintaining healthy Java applications. With the combination of proper thread dump generation techniques and Threadly's advanced analysis features, you can quickly identify and resolve concurrency issues, deadlocks, and performance bottlenecks.

For more advanced Java concurrency patterns and troubleshooting techniques, consider reading "Modern Concurrency in Java" which covers virtual threads, structured concurrency, and performance optimization in detail.

---

*This tutorial covers the essential aspects of thread dump analysis. For specific production scenarios, always test thread dump generation in your environment first.*