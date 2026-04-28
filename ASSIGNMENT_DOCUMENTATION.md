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

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

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
