# CS3701 Operating Systems - Assignment 3: Process Synchronization
## Adding Mutex Locks and Semaphores to CPU Scheduler

### 📋 Assignment Overview

**Deadline:** May 2, 2026 at 11:59 PM | **Grade:** 5% (5 marks) | **Semester:** 2 - AY 2025/2026

This assignment builds upon **Assignment 1** by introducing **process synchronization** concepts. You will identify and fix **race conditions** in a multithreaded CPU scheduler by implementing **mutex locks** and **semaphores**. This assignment focuses on understanding and applying synchronization mechanisms to protect shared resources in concurrent environments.

**What You Will Do:**
- Use the **same codebase** from Assignment 1 (already copied for you)
- Identify race conditions in shared resources (counters, lists, data structures)
- Implement **ReentrantLock** (mutex locks) to protect critical sections
- Implement **Semaphores** to control concurrent access to resources
- Test your synchronization to ensure thread safety
- Document your synchronization strategy and decisions
- Create a video demonstration explaining your synchronization implementation
- Submit through your public GitHub repository with meaningful commits

**Key Requirements:**
- **Video:** 3-5 minutes, uploaded to **personal Gmail** Google Drive (not university email)
- **Commits:** Minimum 4 meaningful commits spread over multiple days
- **Documentation:** Single comprehensive ASSIGNMENT_DOCUMENTATION.md file (worth 2 marks)
- **Repository:** Must be PUBLIC on GitHub with university email account

---

## 🎯 Learning Objectives

By completing this assignment, you will:
- **Understand Race Conditions:** Identify where concurrent threads can cause data corruption
- **Master Mutex Locks:** Learn to use ReentrantLock to create mutually exclusive critical sections
- **Apply Semaphores:** Understand how semaphores control access to limited resources
- **Prevent Deadlocks:** Learn proper lock acquisition and release patterns
- **Ensure Thread Safety:** Make shared data structures safe for concurrent access
- **Debug Concurrency Issues:** Develop skills to identify and fix synchronization problems
- **Practice Professional Development:** Continue building Git/GitHub skills with meaningful commits

---

## 🔍 Background: Understanding Synchronization

### What are Race Conditions?
A **race condition** occurs when multiple threads access shared data simultaneously, and the outcome depends on the timing of their execution. This can lead to incorrect results, data corruption, or program crashes.

**Example from the starter code:**
```java
// RACE CONDITION - Multiple threads can execute this simultaneously!
public static int contextSwitchCount = 0;

public static void incrementContextSwitch() {
    contextSwitchCount++;  // NOT ATOMIC! Read-Modify-Write operation
}
```

If two threads execute `contextSwitchCount++` simultaneously:
1. Thread 1 reads value: 5
2. Thread 2 reads value: 5
3. Thread 1 writes: 6
4. Thread 2 writes: 6

**Result:** Counter is 6 instead of 7! One increment was lost.

### Solution 1: Mutex Locks (ReentrantLock)
A **mutex lock** (mutual exclusion lock) ensures only ONE thread can execute a critical section at a time.

**In Java, use ReentrantLock:**
```java
import java.util.concurrent.locks.ReentrantLock;

private static final ReentrantLock lock = new ReentrantLock();

public static void incrementContextSwitch() {
    lock.lock();  // Acquire lock - only one thread can proceed
    try {
        contextSwitchCount++;  // Critical section - protected!
    } finally {
        lock.unlock();  // Always unlock in finally block!
    }
}
```

### Solution 2: Semaphores
A **semaphore** is a synchronization tool that controls access to a resource pool. It maintains a counter of available permits.

**In Java, use Semaphore:**
```java
import java.util.concurrent.Semaphore;

// Allow only 1 concurrent process to use CPU (binary semaphore)
private static final Semaphore cpuSemaphore = new Semaphore(1);

public void run() {
    try {
        cpuSemaphore.acquire();  // Wait for permit
        // Execute process...
    } catch (InterruptedException e) {
        e.printStackTrace();
    } finally {
        cpuSemaphore.release();  // Return permit
    }
}
```

**Semaphore vs Lock:**
- **Semaphore:** Can allow N threads (permit count). Used for resource pooling.
- **Lock:** Binary (locked/unlocked). Used for mutual exclusion.

---

## 🚀 Getting Started

### Step 1: Fork This Repository on GitHub

Follow these steps to create your own copy of the assignment:

1. **Make sure you're logged in** to GitHub with your **university email** (@std.psau.edu.sa)

2. **Fork this repository**:
   - Go to: `https://github.com/makopt/OS-Assignment3-Starter`
   - Click the **"Fork"** button (top-right corner of the page)
   - GitHub will create a copy under your account
   - Your URL will be: `https://github.com/YOUR_USERNAME/OS-Assignment3-Starter`

3. **Rename your forked repository** (Recommended):
   - In your forked repository, click **Settings** tab
   - At the top, find **"Repository name"**
   - Change name to: `OS-Assignment3-YourFirstName-YourLastName`
   - Example: `OS-Assignment3-Mohammed-Ahmed`
   - Click **"Rename"** button

4. **Make repository PUBLIC**:
   - Still in Settings, scroll down to **"Danger Zone"**
   - Click **"Change visibility"**
   - Select **"Public"** and confirm the change
   - ⚠️ **This is required** - private repositories cannot be graded

### Step 2: Clone Your Repository Using Visual Studio Code

**Option A: Clone Directly from VS Code** (Recommended for beginners)

1. **Open Visual Studio Code**

2. **Open Command Palette:**
   - Windows/Linux: Press **Ctrl+Shift+P**
   - Mac: Press **Cmd+Shift+P**

3. **Type and select:** `Git: Clone`

4. **Paste your repository URL:**
   ```
   https://github.com/YOUR_USERNAME/OS-Assignment3-YourFirstName-YourLastName
   ```
   
5. **Choose a location:**
   - Select a folder on your computer (e.g., `Documents` or `Desktop`)
   - Click **"Select Repository Location"**

6. **Open the cloned repository:**
   - VS Code will ask "Would you like to open the cloned repository?"
   - Click **"Open"**

**Option B: Clone Using Terminal in VS Code**

1. **Open Visual Studio Code**

2. **Open Terminal:**
   - Menu: **Terminal** → **New Terminal**
   - Or keyboard shortcut: **Ctrl+`** (backtick)

3. **Navigate to your desired folder:**
   ```bash
   cd ~/Documents  # Or your preferred location like Desktop
   ```

4. **Clone your repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/OS-Assignment3-YourFirstName-YourLastName
   ```

5. **Open the cloned folder in VS Code:**
   - Menu: **File** → **Open Folder**
   - Navigate to and select the cloned folder
   - Click **"Open"**

**Verify Your Clone:**
   - You should see these files in VS Code Explorer (left sidebar):
     - `SchedulerSimulationSync.java`
     - `ASSIGNMENT_DOCUMENTATION.md`
     - `README.md`
     - `.gitignore`

### Step 3: Set Your Student ID

**⚠️ CRITICAL FIRST STEP ⚠️**

1. **Open** `SchedulerSimulationSync.java` in VS Code

2. **Find line ~278** (use Ctrl+G or Cmd+G to go to line):
   ```java
   // BEFORE:
   int studentID = 123456789;  // CHANGE THIS TO YOUR STUDENT ID!
   ```

3. **Change to your actual student ID:**
   ```java
   // AFTER:
   int studentID = 441234567;  // Replace with YOUR actual student ID
   ```

4. **Save the file:** Press **Ctrl+S** (Windows/Linux) or **Cmd+S** (Mac)

### Step 4: Make Your First Commit Using VS Code

**Using VS Code Source Control:**

1. **Open Source Control panel:**
   - Click the **Source Control** icon in the left sidebar (looks like a branch with 3 circles)
   - Or press **Ctrl+Shift+G** (Windows/Linux) or **Cmd+Shift+G** (Mac)

2. **Stage your changes:**
   - You'll see `SchedulerSimulationSync.java` under "Changes"
   - Click the **+** (plus) button next to the file to stage it
   - The file moves to "Staged Changes"

3. **Write commit message:**
   - In the text box at the top, type: `Set my student ID: 441234567`
   - Press **Ctrl+Enter** (Windows/Linux) or **Cmd+Enter** (Mac) to commit
   - Or click the **✓ Commit** button

4. **Push to GitHub:**
   - Click **"Sync Changes"** button (appears after commit)
   - Or click the **...** menu → **Push**
   - If prompted for credentials, enter your GitHub username and password/token

**Using Terminal (Alternative):**

```bash
git add SchedulerSimulationSync.java
git commit -m "Set my student ID: 441234567"
git push origin main
```

**Verify your commit:**
- Go to your GitHub repository in a web browser
- You should see your commit with the message "Set my student ID..."
- Click on "Commits" to see your commit history

**Verify your commit:**
- Go to your GitHub repository in a web browser
- You should see your commit with the message "Set my student ID..."
- Click on "Commits" to see your commit history

### Step 5: Understand the Starter Code

The starter code (`SchedulerSimulationSync.java`) has the **same functionality** as Assignment 1, but with **intentional race conditions**:

**Shared Resources (needs synchronization):**
1. `SharedResources.contextSwitchCount` - Counter incremented by all threads
2. `SharedResources.completedProcessCount` - Counter incremented when processes finish
3. `SharedResources.totalWaitingTime` - Accumulator for waiting times
4. `SharedResources.executionLog` - ArrayList accessed by all threads

**Your Job:** Add synchronization to make these resources thread-safe!

---

## 📝 Assignment Requirements

This assignment consists of four main tasks. Complete them in order and commit regularly.

### Task 1: Implement Mutex Locks for Counters (1 mark)

**Objective:** Protect shared counters using ReentrantLock

**What to do:**

1. Add import statement:
   ```java
   import java.util.concurrent.locks.ReentrantLock;
   ```

2. In `SharedResources` class, add a lock:
   ```java
   public static final ReentrantLock counterLock = new ReentrantLock();
   ```

3. Protect the three counter methods:
   - `incrementContextSwitch()`
   - `incrementCompletedProcess()`
   - `addWaitingTime()`

4. Use proper lock pattern:
   ```java
   counterLock.lock();
   try {
       // Critical section
   } finally {
       counterLock.unlock();
   }
   ```

**Commit (Using VS Code):**
1. Open Source Control panel (Ctrl+Shift+G)
2. Stage `SchedulerSimulationSync.java` (click + button)
3. Message: `Task 1: Added ReentrantLock for counter protection`
4. Commit (Ctrl+Enter) and Push (Sync Changes)

**Or using Terminal:**
```bash
git add SchedulerSimulationSync.java
git commit -m "Task 1: Added ReentrantLock for counter protection"
git push
```

### Task 2: Implement Mutex Lock for Execution Log (1 mark)

**Objective:** Protect ArrayList from concurrent modifications

**What to do:**

1. In `SharedResources` class, add another lock (or reuse the same):
   ```java
   public static final ReentrantLock logLock = new ReentrantLock();
   ```

2. Protect `logExecution()` method:
   ```java
   public static void logExecution(String message) {
       logLock.lock();
       try {
           executionLog.add(message);
       } finally {
           logLock.unlock();
       }
   }
   ```

3. **Testing:** Run your program multiple times. The log count should be consistent and no `ConcurrentModificationException` should occur.

**Commit (Using VS Code):**
1. Source Control panel → Stage changes → Message: `Task 2: Added ReentrantLock for execution log`
2. Commit and Push

**Or using Terminal:**
```bash
git add SchedulerSimulationSync.java
git commit -m "Task 2: Added ReentrantLock for execution log"
git push
```

### Task 3: Implement Semaphore for CPU Control (1 mark)

**Objective:** Use semaphore to control concurrent process execution

**What to do:**

1. Add import:
   ```java
   import java.util.concurrent.Semaphore;
   ```

2. In `SharedResources` class, add semaphore:
   ```java
   // Binary semaphore - only 1 process can "execute" at a time
   public static final Semaphore cpuSemaphore = new Semaphore(1);
   ```

3. In `Process.run()` method, acquire/release semaphore:
   ```java
   @Override
   public void run() {
       try {
           cpuSemaphore.acquire();  // Wait for CPU
           try {
               // All your process execution code here...
           } finally {
               cpuSemaphore.release();  // Release CPU
           }
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
   }
   ```

4. **Also add to `runToCompletion()` method!**

5. **Experiment:** Try changing `Semaphore(1)` to `Semaphore(2)` - observe the difference!

**Commit (Using VS Code):**
1. Source Control → Stage → Message: `Task 3: Implemented semaphore for CPU control`
2. Commit and Push

**Or using Terminal:**
```bash
git add SchedulerSimulationSync.java
git commit -m "Task 3: Implemented semaphore for CPU control"
git push
```

###Task 4: Complete Documentation (2 marks)

**Complete the single documentation file:** `ASSIGNMENT_DOCUMENTATION.md`

This file includes 6 comprehensive parts:

**📹 IMPORTANT:** Add your video demonstration link in the **highlighted section at the top** of this file!

1. **Part 1: Development Log** - Minimum 3 entries showing progression
2. **Part 2: Technical Questions** - 3 questions about synchronization
3. **Part 3: Synchronization Analysis** - Critical sections and mechanisms
4. **Part 4: Testing and Verification** - Thorough testing with evidence
5. **Part 5: Reflection and Learning** - Demonstrate understanding
6. **Part 6: GitHub Repository Information** - Commits andlinks

**Commit documentation (Using VS Code):**
1. Source Control → Stage `ASSIGNMENT_DOCUMENTATION.md`
2. Message: `Task 4: Completed comprehensive documentation`
3. Commit and Push

**Or using Terminal:**
```bash
git add ASSIGNMENT_DOCUMENTATION.md
git commit -m "Task 4: Completed comprehensive documentation"
git push
```

---

## 🎥 Video Demonstration Requirements (Required - see grading)

Create a **video (maximum 5 minutes)** showing:

### What to Include:

1. **Introduction (30 sec):**
   - Your name and student ID
   - GitHub repository (show it's public)
   - Briefly explain this is Assignment 3 on synchronization

2. **Code Walkthrough - Synchronization Mechanisms (90 sec):**
   - Open `SchedulerSimulationSync.java` in your IDE
   - Show where you added ReentrantLock declarations
   - Explain ONE critical section you protected (e.g., `incrementContextSwitch()`)
   - Show where you added Semaphore
   - Explain how the semaphore controls CPU access

3. **Synchronization Explanation (90 sec):**
   - **Explain** what race conditions COULD occur without synchronization (theoretical)
   - Show where shared resources are accessed (counters, ArrayList)
   - **Explain** why concurrent access is dangerous (even if it doesn't always fail)
   - Run the program with your synchronization and show consistent correct results
   - Explain how your locks/semaphores prevent potential race conditions

4. **Commits & Conclusion (30 sec):**
   - Show your commit history (at least 4 commits)
   - Briefly state what you learned about synchronization

### Video Technical Requirements:
- **Length:** Maximum 5 minutes (aim for 4-5 minutes)
- **Upload:** Google Drive with sharing set to "Anyone with the link"
  - ⚠️ **IMPORTANT:** Use your **PERSONAL Gmail account** for Google Drive upload
  - **DO NOT** use your university email (@std.psau.edu.sa) for Google Drive
  - Reason: University Google Workspace may have restrictions or expire after graduation
- **Quality:** Clear screen recording + audio narration
- **File naming:** `StudentID_Assignment3_Synchronization.mp4`
- **Add link:** Put Google Drive link in your README.md
- **Test access:** Open link in incognito/private window to verify anyone can view

**Recording Tools:**
- **Mac:** QuickTime Player (File → New Screen Recording)
- **Windows:** Xbox Game Bar (Win + G) or OBS Studio (free)
- **Linux:** SimpleScreenRecorder or OBS Studio

---

## 📊 Grading Rubric (Total: 5 marks)

### Task 1: Mutex Locks for Counters (1 mark)
- ✓ ReentrantLock declared correctly (0.3)
- ✓ All three counter methods protected (0.9)
- ✓ Proper lock/unlock pattern with finally block (0.3)

### Task 2: Mutex Lock for Execution Log (1 mark)
- ✓ Lock declared for log protection (0.3)
- ✓ logExecution() method properly protected (0.9)
- ✓ No concurrent modification exceptions (0.3)

### Task 3: Semaphore Implementation (1 mark)
- ✓ Semaphore declared correctly (0.3)
- ✓ acquire() and release() in run() method (0.6)
- ✓ Also implemented in runToCompletion() (0.3)
- ✓ Semaphore in finally block to prevent leaks (0.3)

### Task 4: Documentation (2 marks)
- ✓ **Video link added** in highlighted section at top of ASSIGNMENT_DOCUMENTATION.md (Required!)
- ✓ ASSIGNMENT_DOCUMENTATION.md completed with all 6 parts (1.0)
- ✓ Development log shows progression (0.3)
- ✓ Technical questions answered comprehensively (0.3)
- ✓ Testing documented with evidence (0.2)
- ✓ Reflection shows understanding (0.2)

### Professional Practices (Points integrated above):
- ✓ Minimum 4 meaningful commits
- ✓ Commits spread over time (not all at once)
- ✓ Student ID set correctly
- ✓ Code compiles and runs without errors

### Video Demonstration (Required - affects overall grade):
- Video is mandatory and demonstrates your understanding
- Must be uploaded to **personal Gmail** Google Drive
- Missing or inaccessible video: -2 marks from total
- See penalties section below for other video-related deductions

---

## ⚠️ Penalties

### Late Submission:
- **1 day late (within 24 hours):** -1 mark
- **2 days late (24-48 hours):** -2 marks  
- **More than 2 days late:** Assignment not accepted (0 marks)

### Video Issues:
- **Missing video:** -2 marks
- **Video not accessible (private/restricted link):** -2 marks
- **Video exceeds 5 minutes:** -0.5 marks
- **Video less than 3 minutes (insufficient content):** -1 mark
- **No audio explanation:** -1 mark
- **Does not show commit history:** -0.5 marks
- **No explanation of synchronization concepts:** -0.5 marks

### Repository Issues:
- **Private repository (not public):** -0.5 marks
- **Repository not accessible:** Cannot grade (0 marks until fixed)
- **Not using university email for GitHub account:** -0.25 marks
- **Repository name not descriptive:** -0.25 marks

### Code Quality Issues:
- **Code does not compile:** -2 marks
- **Code does not run:** -1.5 marks
- **Student ID not set or incorrect:** -0.5 marks
- **Missing try-finally blocks:** -0.5 marks per missing block (serious safety issue!)
- **Race conditions still present (not fixed):** -1 mark per unfixed race condition

### Commit and Documentation Issues:
- **Single commit or bulk commits (all at once):** -0.5 marks
- **Less than 4 commits:** -0.25 marks per missing commit
- **Commits made after deadline (inspected via git history):** Late penalty applies
- **Empty or meaningless commit messages:** -0.25 marks
- **ASSIGNMENT_DOCUMENTATION.md incomplete:** Proportional deduction from Task 4 marks

### Academic Integrity Violations:
- **Plagiarism (code or documentation):** 0 marks + academic misconduct report for all parties involved
- **AI-generated code without understanding:** 0 marks if unable to explain during interview
- **Fake commit history (manipulated timestamps):** 0 marks + academic misconduct report
- **Copied from classmate:** 0 marks for both students + academic misconduct report

### Important Notes:
- Penalties are cumulative (multiple violations = multiple deductions)
- Maximum total deduction cannot exceed 5 marks (floor is 0)
- In case of legitimate technical issues, contact instructor **BEFORE** deadline
- Video accessibility will be tested - ensure "Anyone with the link" can view

---

## ⚠️ Common Mistakes to Avoid

1. **Forgetting finally block:**
   ```java
   // WRONG - if exception occurs, lock never released!
   lock.lock();
   criticalSection();
   lock.unlock();
   
   // CORRECT
   lock.lock();
   try {
       criticalSection();
   } finally {
       lock.unlock();
   }
   ```

2. **Not releasing semaphore:**
   ```java
   // WRONG
   semaphore.acquire();
   doWork();
   semaphore.release();  // If doWork() throws exception, never released!
   
   // CORRECT
   semaphore.acquire();
   try {
       doWork();
   } finally {
       semaphore.release();
   }
   ```

3. **Inconsistent locking:**
   - If you protect one counter, protect ALL counters!
   - Use the same lock for related data

4. **Deadlock:**
   - Always acquire locks in the same order
   - Don't hold locks longer than necessary

---

## 🧪 Testing Your Code

### How to Run in Visual Studio Code

**Option 1: Using VS Code Terminal** (Recommended)

1. Open Terminal in VS Code (Terminal → New Terminal or Ctrl+`)
2. Make sure you're in the project directory (you should see the .java file)
3. Compile:
   ```bash
   javac SchedulerSimulationSync.java
   ```
4. Run:
   ```bash
   java SchedulerSimulationSync
   ```

**Option 2: Using VS Code Java Extension**

1. Install "Extension Pack for Java" by Microsoft (if not already installed)
2. Open `SchedulerSimulationSync.java`
3. Click the **▶️ Run** button that appears above the `main` method
4. Or right-click in the editor → **Run Java**

### Test 1: Run Multiple Times for Consistency

Run your program **at least 3 times** and compare the statistics:

**Using Terminal:**
```bash
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**What to check:**
- Context switches count should be **identical** every time
- Completed processes count should be **identical**
- Total waiting time should be **identical**
- Average waiting time should be **identical**

**If numbers vary**, you have race conditions that need fixing!

### Test 2: Check log count
The execution log count should equal: `(context switches) + (process completions)` approximately

### Test 3: Verify no exceptions
Run at least 5 times - should NEVER see:
- `ConcurrentModificationException`
- Deadlocks (program hangs)
- Negative counters

---

## 📚 Additional Resources

- **Java ReentrantLock:** https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/ReentrantLock.html
- **Java Semaphore:** https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html
- **Operating Systems Concepts (Silberschatz):** Chapter 6 - Synchronization
- **Java Concurrency in Practice** (recommended reading)

---

## 🔖 Repository Structure

```
OS-Assignment3-Starter/
|-- SchedulerSimulationSync.java    (Main code - ADD SYNCHRONIZATION HERE)
|-- ASSIGNMENT_DOCUMENTATION.md      (Complete this single file with all documentation)
|-- README.md                       (This file - instructions)
|-- .gitignore                      (Configured for Java)
```

---

## 📤 Submission Instructions

### What to Submit on Blackboard:

**Submit ONLY your GitHub repository link directly on Blackboard.**

**Format:**
```
https://github.com/[your-username]/OS-Assignment3-[YourName]
```

**Example:**
```
https://github.com/mohammed-ahmed-441/OS-Assignment3-Mohammed-Ahmed
```

### 🎥 Where to Put Your Video Link:

**⚠️ IMPORTANT:** Your video demonstration link must be added to the **`ASSIGNMENT_DOCUMENTATION.md`** file in your repository.

**Steps:**
1. Upload your 3-5 minute video to **PERSONAL Gmail Google Drive** (NOT @std.psau.edu.sa)
2. Set sharing to "Anyone with the link can view"
3. Test the link in incognito/private browser mode
4. Copy the link
5. Open `ASSIGNMENT_DOCUMENTATION.md` in your repository
6. Paste the link in the **highlighted VIDEO DEMONSTRATION LINK section** at the top
7. Commit and push this change to GitHub

**Video Requirements:**
- **Duration:** 3-5 minutes (less than 3 min: -1 mark, more than 5 min: -0.5 mark)
- **Platform:** Personal Gmail Google Drive ONLY (not university email)
- **Filename:** `[YourStudentID]_Assignment3_Synchronization.mp4`
- **Content:** Code walkthrough, synchronization explanation, commits demonstration
- **Audio:** Must have clear audio (-1 mark for no audio)
- **Access:** Must be accessible to anyone with link (test in incognito mode)

**Why Personal Gmail?**
- University email accounts may have restrictions or expire
- Personal Gmail ensures long-term accessibility for grading
- DO NOT use @std.psau.edu.sa for Google Drive

---

## ✅ Final Checklist

Before submission, verify:

- [ ] Repository is **PUBLIC**
- [ ] Student ID set in code
- [ ] Code compiles: `javac SchedulerSimulationSync.java`
- [ ] Code runs successfully: `java SchedulerSimulationSync`
- [ ] All locks have try-finally blocks
- [ ] Semaphore acquire/release in finally blocks
- [ ] All counters protected
- [ ] Execution log protected
- [ ] Minimum 4 commits with good messages
- [ ] Commits spread over different days
- [ ] ASSIGNMENT_DOCUMENTATION.md completed (all 6 parts)
- [ ] **Video link added to ASSIGNMENT_DOCUMENTATION.md** (in highlighted section at top)
- [ ] Video is 3-5 minutes long
- [ ] Video uploaded to **personal Gmail** Google Drive (NOT @std.psau.edu.sa)
- [ ] Video link works (test in incognito mode)
- [ ] Video sharing set to "Anyone with the link"
- [ ] GitHub account uses university email (@std.psau.edu.sa)
- [ ] Everything pushed to GitHub
- [ ] **Repository link submitted on Blackboard**

---

## 🎓 Academic Integrity

- **AI-Generated Code:** Must demonstrate understanding. You will be asked to explain your synchronization choices.
- **Plagiarism:** Zero marks for all parties involved
- **Commit History:** Shows your development process - must be authentic
- **Video:** Must explain concepts in your own words

---

**Good luck! This assignment will deepen your understanding of concurrent programming and synchronization - essential skills for any software developer!**

For questions about this assignment, contact your instructor or post in the course discussion forum.
