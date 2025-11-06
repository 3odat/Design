## 🧠 What “Reads from Perception Output” Means

Your perception system already publishes data through APIs like:

```
/perception/scene
/perception/history
/fusion/scene
```

When we say the **controller/mission agent “reads from” perception output**, it simply means:

➡️ The controller (the decision-making agent) **requests data** from these endpoints to understand the current world.

For example:

```python
scene = requests.get("http://localhost:8090/perception/scene?drone=drone1").json()
```

Now the controller knows:

> “Drone 1 sees a car ahead and an obstacle to the left.”

That’s “reading from” the perception output — just like a pilot reading sensors before making a move.

---

## 🔁 What “Subscribes to Perception Output” Means

This is the same idea, but **in real-time**.

Instead of asking again and again (“what’s the scene now?”),
the controller **subscribes** to a stream that pushes updates automatically.

That’s what your `/stream/scene?drone=drone1&hz=10` does —
it sends updates 10 times per second.

So, “subscribing” means the controller stays connected and receives continuous updates like:

```
Time: 12.00 → Car at (3.1, -1.2)
Time: 12.10 → Car at (3.4, -1.1)
Time: 12.20 → Car at (3.8, -1.0)
```

Now, the controller can **react immediately** to changes —
without needing to ask every time.

---

## 🎮 Why This Matters

The controller (the “brain”) uses perception data in one of two modes:

| Mode                      | Description                                                    | Example Use                                         |
| ------------------------- | -------------------------------------------------------------- | --------------------------------------------------- |
| **Read (polling)**        | The controller asks the perception API only when it needs data | “Before takeoff, check the area is clear.”          |
| **Subscribe (streaming)** | The controller gets continuous updates as the scene changes    | “While flying, keep tracking the car in real time.” |

Both are valid — they just depend on how reactive or continuous the task is.

---

## 🧩 In Your System

Since your perception layer already supports both modes:

* `/scene` → for **read** access,
* `/stream/scene` → for **subscribe** (live updates),

your mission agent (or control loop) can decide which one to use depending on the mission type.

Example:

* If it’s a slow inspection task → just read `/scene`.
* If it’s target tracking → subscribe to `/stream/scene`.

---

Would you like me to show a small **Python sketch** of how the agent “reads” vs. “subscribes” to your perception layer, so you can see what it looks like in practice?
