# Inter-Process Communication (IPC) in Linux  

## 1. What is IPC (Inter-Process Communication)?

IPC allows **separate processes** to:
- Exchange data
- Synchronize execution
- Coordinate work

Processes **do NOT share memory by default**. IPC exists because isolation is strict.

### Common IPC Mechanisms
| Mechanism | Speed | Complexity | Use Case |
|--------|------|-----------|--------|
| Pipes | Fast | Simple | Parent-child communication |
| FIFOs | Medium | Simple | Unrelated processes |
| Message Queues | Medium | Medium | Structured messaging |
| Shared Memory | Fastest | Complex | High-performance systems |
| Sockets | Flexible | Complex | Network & local IPC |

---

## 2. Pipes (Anonymous Pipes)

### What a Pipe Really Is
A pipe is:
- A **kernel buffer**
- With **two file descriptors**
- One for reading, one for writing

```
fd[0]  ---> READ END
fd[1]  ---> WRITE END
```

### Key Rules (Memorize These)
- Pipes are **unidirectional**
- Pipes work **ONLY between related processes**
- Pipe must be created **BEFORE fork()**
- Pipes die when processes exit

### Creating a Pipe
```c
int fd[2];
pipe(fd);
```

### Why pipe() Must Be Before fork()
Because `fork()` **copies the file descriptor table**.  
No pipe before fork = no shared channel.

---

## 3. Blocking Behavior (CRITICAL)

### Reads
- If pipe is empty → `read()` **blocks**
- If writer closes pipe → `read()` returns `0` (EOF)

### Writes
- If buffer is full → `write()` **blocks**
- If no reader exists → **SIGPIPE signal** (program crashes)

### SIGPIPE (People Forget This)
```c
write(fd, msg, len); // crashes if no reader
```
To avoid crash:
```c
signal(SIGPIPE, SIG_IGN);
```

---

## 4. Pipe Atomicity (Hidden Exam Topic)

Writes **≤ PIPE_BUF (usually 4096 bytes)** are atomic.  
Larger writes **can interleave**.

| Write Size | Safe? |
|----------|------|
| ≤ 4096 bytes | YES |
| > 4096 bytes | NO |

---

## 5. Bidirectional Communication

### Why One Pipe Fails
A single pipe:
- One direction
- Causes deadlock if both try to read/write

### Solution
Use **TWO pipes**
```
Parent → Child
Child → Parent
```

This is the correct architecture. Anything else is wrong.

---

## 6. Named Pipes (FIFOs)

### What Makes FIFOs Different
- Exist as **files**
- Can be accessed by **any process**
- Persist until `unlink()`

### Creating a FIFO
```c
mkfifo("/tmp/myfifo", 0666);
```

### FIFO Permissions
Same as files:
- 0666 → anyone can read/write
- Controlled by umask

---

## 7. FIFO Blocking Behavior (IMPORTANT)

| Open Mode | Behavior |
|--------|---------|
| O_RDONLY | Blocks until writer opens |
| O_WRONLY | Blocks until reader opens |
| O_NONBLOCK | Returns immediately |

### Deadlock Scenario
Both processes open FIFO for writing first → **deadlock**

Correct order:
- One opens read first
- Other opens write

---

## 8. FIFO Chat Architecture (Alice & Bob)

You did this right — TWO FIFOs are mandatory.

```
Alice writes → fifo_alice → Bob reads
Bob writes → fifo_bob → Alice reads
```

One FIFO chat = amateur mistake.

---

## 9. File Descriptor Inheritance

After fork():
- Parent & child have SAME fd numbers
- Closing fd in one process does NOT close it in the other

This is why unused ends **MUST be closed**.

---

## 10. Closing Unused Ends (Deadlock Killer)

### Why It Matters
If writer fd remains open:
- Reader never gets EOF
- Program hangs forever

### Rule
> If you don’t use it, CLOSE IT.

---

## 11. Reading & Writing Safely

### WRONG
```c
read(fd, buffer, 200);
```

### RIGHT
```c
read(fd, buffer, sizeof(buffer));
```

Always respect buffer boundaries. Overflow = undefined behavior.

---

## 12. String Handling (Beginner Trap)

### fgets() Adds Newline
```c
fgets(buf, 100, stdin); // includes '\n'
```

### Correct Fix
```c
buf[strcspn(buf, "\n")] = '\0';
```

Failing this makes string comparisons break.

---

## 13. Error Handling (You Must Do This)

### ALWAYS Check Return Values
```c
if (pipe(fd) == -1) perror("pipe");
if (fork() == -1) perror("fork");
if (read(fd, buf, n) == -1) perror("read");
```

Ignoring errors = fake understanding.

---

## 14. Cleaning Up (Be Professional)

### Pipes
- Automatically cleaned on process exit

### FIFOs
- MUST be manually removed
```c
unlink("/tmp/myfifo");
```

Leaving junk in /tmp is sloppy.

---

## 15. When NOT to Use Pipes/FIFOs

Do NOT use them if:
- You need structured messages → Message Queues
- You need speed → Shared Memory
- You need networking → Sockets

Wrong tool = bad design.

---

## 16. Summary (Burn This Into Your Brain)

- pipe() before fork()
- Close unused ends
- One pipe = one direction
- Two pipes = two-way
- FIFOs block unless handled carefully
- Always handle SIGPIPE
- Respect buffer sizes

If you understand all this, you're no longer a beginner.

---

## 17. Next Level (Optional but Smart)
Study:
- select(), poll(), epoll()
- Shared memory + semaphores
- UNIX domain sockets

That’s how real systems are built.
