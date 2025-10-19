
---

## 2 — Concurrency Visualizer (`src/concurrency_visualizer.py`)

**Code**
```python
#!/usr/bin/env python3
"""
Concurrency Visualizer (thread/async simulation)
- Simulates tasks, collects timeline events, outputs JSON timeline for visualization
Run: python3 src/concurrency_visualizer.py
"""
import time, threading, json, random
from collections import deque

timeline = deque()

def mark(event: str, meta=None):
    timeline.append({"ts": time.time(), "event": event, "meta": meta or {}})

def worker(name, iterations=5):
    for i in range(iterations):
        mark("start_task", {"worker": name, "iter": i})
        # simulate variable work
        time.sleep(random.random() * 0.2)
        mark("end_task", {"worker": name, "iter": i})

def demo():
    threads = [threading.Thread(target=worker, args=(f"w{i}",)) for i in range(4)]
    for t in threads: t.start()
    for t in threads: t.join()
    # dump timeline
    events = list(timeline)
    print("Timeline events:", len(events))
    print(json.dumps(events, indent=2))

if __name__ == "__main__":
    demo()
