# Assignment 3 - Complete Documentation

**Student Name**: [Your Full Name]  
**Student ID**: [Your ID]  
**Date Submitted**: [Submission Date]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [April 24, 2026 and  5:00 PM]
**What I implemented**: 
I worked on tack1 and added a ReentrantLock in the SharedResources class. I used it to lock the three counters, contextswitchcount, completedprocesscount, and totlwaitingtime.
**Challenges encountered**: 
The code was freezing up when i first ran it. I realized the threads were getting stuck because the lock wasn't opening back up properly after the first use.
**How I solved it**: 
I used a try- finally block for the counters.I put lock()
at the start and unlock() in the finally part so it always releases.The fixed the freeze and now the program runs smooth.
**Testing approach**: 
I ran the program about 4 times, The number of finshed operation -16- and log entries -62- were identical each time. I detected very minor changes in total waiting time -just a few milliseconds- which is normal given how the os handles threads, But the oveall result are consistent and accurate.
**Time spent**: 
40 minutes
---

### Entry 2 - [April 25, 2026 and  11:00 PM]
**What I implemented**: 
I just added a  ReentrantLock to the SharedResources part. I need it to protect the executionlog and those shared counters.
**Challenges encountered**: 
The issue was with the Arraylist because it's not thread safe. If wo threads try to add somthing at the same time, The program just crashes with a ConcurrentModificationException error.

**How I solved it**: 
I used lock.lock() before any that touches the shared data. Anyway, I put lock.unlock() in a finally blocks so the program won't freez up if somthing goes wrong.
**Testing approach**: 
I ran the code mant times, I just checked the terminal to make sure there are no red errors and the logs are correct.
**Time spent**: 
30 minutes
---

### Entry 3 - [April 27, 2026 and  9:50 PM]
**What I implemented**: 
I added a Semaphore to SharedResources. Then in the Process class 
I called acquire() before the process runs and release() after it, Finishes so only one process touches the cpu at a time.
**Challenges encountered**: 
I didnt know where exactly release() should go. I throught what if the thread crashes mid execution, The semaphore stays taken and nothing else can run after that.
**How I solved it**: 
I remembered what i did in ernty 1 and 2 with the lock, so I just did the same thing and threw release() in the finally block, Worked fine after that
**Testing approach**: 
Ran it like 5 or 6 times, Context switches were -31- every time and completed proceses always -16-. No crashes no freezing so looks good.
**Time spent**: 
45 minutes
---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[Race Condition-1-shared Counters: The first race condition I found with the shared counters like contextswitchcount.The issue is that when you write contextswitchcount++ it looks simple but is actually 3 steps,read the value,add 1,then write it back.So if two threads are running at the same time and both read the value as 5,they will both write 6 back.We lost a count.This will make the statistics at the end show wrong number
Example code java:
//thread 1 reads 5
//thread 2 also reads 5 at same time
//both write 6, but it should be 7
contextSwitchCount++;

Race Condition-2-Executionlog:The second is the executilong Arraylist.Arraylist in java is not thread safe,It was never made to handle multiple threads adding to it at the same time.If two threads call add() together the internal array can get messed up and you might lose entries or get an exception thrown.
Example code java:
executionLog.add(message); // two threads here at same time = problem.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[So basically a ReentranLock is used when you want only one thread inside a section at a time,Its for mutual exclusion,The reentrant part just means the same thread can lock it again without getting stuck.A Semphore is a bit different,It controls how many thrads can access somthing at once using permits.
I used the ReentranLock to protect the counters and the Arraylist because I just need one thread modifying them at a time,Nothing complicated,
 code java:
 lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}
For the cpu access I used Semaphore(1) because it make more sens conecptually,The cpu is a resource with limited slots and the semphore represents that.Also if we ever wanted to simulate 2 cpus we just change the number to 2,Way easier than changing a lock.
code java:
SharedResources.cpuSemaphore.acquire();
//running on cpu
SharedResources.cpuSemaphore.release();]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Deadlock is basically when two threads get stuck waiting for each other and neither one can move forward. For example thread A is holding lock 1 and waiting for lock 2, but thread B is holding lock 2 and waiting for lock 1. So they both just sit there forever and the program freezes.
Technique 1-finally block:
Waht I did here is always put the lock.unlock() inside a finally block.The is if somthing goes wrong and an exception happens in the middle of the code,The lock will still get released.If didnt do this and the code crashed while holding the lock,Every other thread waiting for that lock would be stuck forever.
code java:
lock.lock();
try {
    completedProcessCount++;
} finally {
    lock.unlock();
}
Technique 2-one lock for everything:
Instead of creating a different lock for each counter I just used one lock for all shared resources.Deadlocks usually happen when threads are grabbing multiple locks doing it in different orders,So they end up waiting on each other.Since I only have one lock in the whole program that problem cant happen because there is nothing to create a circular wait between threads.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextswitchcount,completedprocesscount,totalwaitingtime
**Why they need protection**: 
These three variables get updated by different threads running at the same time. The issue is that incrementing a variable is not one step, the thread reads it, adds to it, then saves it back.Two threads doing this together will both read the same old number and one change gets lost.This breaks the counts completely.
**Synchronization mechanism used**: 
Reentrantlock
**Code snippet**:
```java
 public static void incrementContextSwitch() {
        lock.lock(); // Acquire a lock to avoid race coditions during context switch incremen 
        try {
            contextSwitchCount++;
        } finally{
            lock.unlock();
        }
        
    }
    
    // Method to increment completed process counter
    public static void incrementCompletedProcess() {
        lock.lock(); // protect shared counter for completed processes 
        try {
             completedProcessCount++;
        } finally{
             lock.unlock();
        }
    }
    
    // Method to add waiting time
    public static void addWaitingTime(long time) {
        // TODO: 
        lock.lock();// Synchronize access to the overall waiting time accumulator 
        try {
             totalWaitingTime += time;
        } finally{
         lock.unlock();
        }
       
    }
```

**Justification**: 
One thread locks,Update the variable,Then unlock.The next thread can only go in after that.The fnally block mackes sure the lock alawys gets released no matter what happens.
---

### Critical Section #2: Execution Log

**What resource**: 
The executionlog which is an Arraylist of string.
**Why it needs protection**: 
The problem is that Java's Arraylist isn't thread-safe.When multiple threads try to call .add() at the exact same time,The list's internal structure can break.This leads to missing log entries or the program crashing a ConcurrentModificationException error.
**Synchronization mechanism used**: 
Reentrantlock
**Code snippet**:
```java
public static void logExecution(String message) {
        lock.lock(); // Acquire the lock to protect the Arrylist
        try{
        executionLog.add(message);
        }
        finally{
            lock.unlock();
        }
}
    
```

**Justification**: 
I used a lock to wrap the add method so only one thread can write to the log at a time.Putting the unlock() in a finally block is a safety measure,It mackes sure the lock is always released even if an error happens,Which prevents the whole simulation from getting stuck in a deadlock. 
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
My goal was making sure only one process thread touches the cpu logic at a time,Since we only have one cpu here.
**Number of permits and why**: 
I gave it 1 permit.Having more than 1 would let multiple processes run together which is wrong for this simulation.
**Where implemented**: 
Both run() and runtocompletion() aquire it at the start and release it in finally.
**Code snippet**:
```java
 //This ensures only one process thread can simulate CPU access at a time
    public static final Semaphore cpuSemaphore = new  Semaphore(1);

    try { 
          SharedResources.cpuSemaphore.acquire();
   
   }
     catch (InterruptedException e1) {
        Thread.currentThread().interrupt(); // Signal to the calling thread that an interruption occurred during semaphore acquisition

        }  finally {  SharedResources.cpuSemaphore.release();

        }
    
```

**Effect on program behavior**: 
Each thread waits its turn at the semphore.When the running one finishes and release,The next waiting thread goes in,Processes end up running one after another which is exactly what round robin needs
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: 
Consistency Check
**Testing procedure**: 
```bash
 Run: SchedulerSimulationSync
 5 times
```

**Results**: 
(Run:   Contextswitches  completedprocesses  Totalwaitingtime    Logentries
  1:        31                16                  743610ms         62
  2:        31                16                  743600ms         62
  3:        31                16                  743493ms         62
  4:        31                16                  742737ms         62
  5:        31                16                  742887ms         62 
  )

**Why synchronization is necessary**: 
(Okay so basically without the lock,Two threads can read the same variable at the same time and both try to update it.Like if contextswitchescount is 5 and two threads read it at the same time,They both make it 6 instead of 7.We lose an increment.Same thing with the Arraylist,Its not thread safe so two threads adding to it at the same time can mess it up or even crash the program.)

**Conclusion**: 
Every run gave the same contextswitches-31-,Same completed processes-16-,And same log enties-62-.The waiting time changed by a few ms each time but thats just normal timing differences from the os,Not a real problem.
---

### Test 2: Exception Testing
**What I tested**: 
Basically I was checking if the program would break or throw any errors when threads are running together,Like ConcurrentModificationException since multiple threads share the same Arraylist.
**Testing procedure**: 
Ran it 5 times and just kept an eye on the terminal output
**Results**: 
Nothing broke.All 5 runs went through fine with no errors showing up.Logentries stayed at -62- every time so nothing got lost either.
**What this proves**: 
So the reason no exception showed up is because only one thread can write to the log list at a time.If we remove the lock,Things would go wrong pretty fast since threads dont wait for each other and the list would get hit from multiple sides at once.Seeing -62- entries every run with zero crashes was enough for me to confirm the locking mechanism is holding up fine.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (Checking if the final numbers actually make sense logically)
**Expected values**: 
Since we use the student id as a random seed the processes never change between runs,So we should always get -16- completed processes.Contextswitches should be -31-Since thats how many times run() gets called total.And logentrie should be 62 because each quantum writes exactly 2 messages. 
**Actual values**: 
1-CompletedProcesses: 16 
2-ContextSwitches: 31 
3-LogEntries: 62 
4-all burst times were the same in every run 
**Analysis**: 
Everything matched what i expected.No numbers were off,Nothing was missing.The locks are clearly preventing ane race conditions from messing up the counters. 
---

### Test 4: Different Scenarios
**Scenario tested**: [understanding why waiting time changes slightly each run even though everything else stays the same.]

**Purpose**: 
I wanted to figure out if the small waiting time differences mean something is wrong or if its just normal
**Results**: 
The number of processes and their burst times never changed because theyre generated from a fixed seed.But waiting time uses System.currentTimeMillis() which is real clock time,So it naturally varies a little.For example P1 had waiting times between 20ms and 29ms across the 5 runs.
**What I learned**: 
The small waiting time differences are totally normal and dont mean anything is broken.If there was actually a race condition happening,The contextswitchcount or completedprocesscount would be wrong in some runs.That never happened so the synchronization is working fine.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
