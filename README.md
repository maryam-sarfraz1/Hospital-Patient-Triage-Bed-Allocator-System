# 🏥 Hospital Patient Triage & Bed Allocator

**A multi-process, multi-threaded hospital simulation built to demonstrate core Operating Systems concepts — process management, IPC, synchronization, and memory allocation.**

`CL2006 – Operating Systems Lab | Spring 2026`

![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![POSIX](https://img.shields.io/badge/POSIX-Threads-blue?style=for-the-badge)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)

---

## 🏛️ About

The **Hospital Patient Triage & Bed Allocator** simulates the admission, triage, and discharge pipeline of a hospital using real OS primitives — processes, threads, pipes, shared memory, and synchronization tools — to model concurrent patient flow and dynamic bed allocation under constrained resources (ICU, Isolation, General wards).

---

## 🚀 Build & Run

```bash
make all          # Compile everything
make run          # Start hospital (default: best-fit)
make test         # Run stress test (20 patients)
make clean        # Remove binaries and IPC artifacts
```

### ⚙️ Start with a Specific Allocation Strategy

```bash
./scripts/start_hospital.sh --strategy best    # Best-Fit (default)
./scripts/start_hospital.sh --strategy first   # First-Fit
./scripts/start_hospital.sh --strategy worst   # Worst-Fit
```

### 🩺 Admit a Patient

```bash
./scripts/triage.sh <name> <age> <severity 1-10>
./scripts/triage.sh Ali 25 8
```

### 🛑 Shutdown

```bash
./scripts/stop_hospital.sh
```

---

## 🧠 OS Concepts Demonstrated

| Concept | Where |
|---|---|
| 🍴 `fork()` + `execv()` | `admissions.c` spawns `patient_sim` per patient |
| 👻 `SIGCHLD` + `waitpid(WNOHANG)` | Zombie reaping in `admissions.c` |
| 🔗 Anonymous Pipe | `triage.sh` → `admissions` (patient data) |
| 📡 Named FIFO | `patient_sim` → `admissions` (discharge) |
| 🧩 Shared Memory (`shmget`) | Ward bitmap shared across processes |
| 🧵 POSIX Threads | Receptionist, Scheduler, 3 Nurse threads |
| 🔒 Mutex | Protects bed bitmap from race conditions |
| 📶 Condition Variable | Scheduler waits on `bed_freed` signal |
| 🚦 Semaphores | Enforces ICU = 4, ISO = 4 capacity limits |
| 🔄 Producer–Consumer | Receptionist enqueues, Scheduler dequeues |
| 📦 Best-Fit Allocator | Selects smallest fitting bed partition |
| 🧬 Coalescing | Merges adjacent free beds after discharge |
| 📊 Fragmentation Report | Logged after every alloc/dealloc |
| 🗂️ Paging Simulation | Internal fragmentation per patient |

---

## 📁 File Structure
