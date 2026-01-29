#  - Monolithic Architecture

NAME : BHAVANI | **SRN:** PES1UG23CS144 | **Course:** Cloud Computing Lab

---

## 📋 About This Lab

This lab demonstrates **Monolithic Architecture** using a college fest management application built with FastAPI. The application shows both advantages (simplicity, easy development) and disadvantages (single point of failure, scaling issues) of monolithic systems.

**Key Features:** User registration, login, event listing, event registration, checkout

---

## 🚀 Quick Setup
```bash
# 1. Clone and navigate
git clone https://github.com/YOUR-USERNAME/CC-Lab2-Monolithic-Architecture.git
cd CC-Lab2-Monolithic-Architecture

# 2. Create virtual environment
python3 -m venv .venv
source .venv/bin/activate  # Windows: .\.venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Initialize database
python insert_events.py

# 5. Run server
uvicorn main:app --reload

# 6. Access application
# Register: http://localhost:8000/register
# Login: http://localhost:8000/login
# Events: http://localhost:8000/events
```

---

## 📁 Project Structure
```
├── main.py                    # Main application with all routes
├── database.py                # Database connection
├── insert_events.py           # Database seeding script
├── requirements.txt           # Dependencies
├── checkout/__init__.py       # Checkout logic
├── locust/                    # Load testing files
│   ├── checkout_locustfile.py
│   ├── events_locustfile.py
│   └── myevents_locustfile.py
└── templates/                 # HTML templates
```

---

## 🧪 Load Testing
```bash
# Keep server running, open new terminal

# Test any route
locust -f locust/checkout_locustfile.py
locust -f locust/events_locustfile.py
locust -f locust/myevents_locustfile.py

# Open UI: http://localhost:8089
# Settings: Host=http://localhost:8000, Users=1, Ramp-up=1, Time=30s
```

---

## ⚡ Optimizations Performed

### 1. **Checkout Route** (`checkout/__init__.py`)
**Problem:** Inefficient while loop counting down fees one by one  
**Before:**
```python
for e in events:
    fee = e[0]
    while fee > 0:
        total += 1
        fee -= 1
```
**After:**
```python
total = sum(e[0] for e in events)
```
**Impact:** Reduced complexity from O(n×m) to O(n), ~5% faster response time

---

### 2. **Events Route** (`main.py` lines 65-67)
**Problem:** Wasteful loop running 3,000,000 iterations  
**Solution:** Removed unnecessary computation loop  
**Impact:** Significantly faster response time

---

### 3. **My-Events Route** (`main.py` lines 101-103)
**Problem:** Wasteful loop running 1,500,000 iterations  
**Solution:** Removed unnecessary computation loop  
**Impact:** Significantly faster response time

---

## 🔴 Key Demonstration: Monolithic Failure

- Introduced bug in `/checkout` route that **crashed entire application**
- All routes became unavailable due to one module's failure
- **This is the main disadvantage of monolithic architecture** - no fault isolation
- In microservices, individual services can fail without bringing down the entire system

---

## 📊 Results Summary

| Route | Optimization | Result |
|-------|-------------|---------|
| `/checkout` | Removed nested loops | ~5% faster |
| `/events` | Removed 3M iteration loop | Significantly faster |
| `/my-events` | Removed 1.5M iteration loop | Significantly faster |

**All performance improvements verified using Locust load testing.**

---

## 🛠️ Tech Stack

FastAPI • SQLite • Locust • Jinja2 • Uvicorn

---

## ✅ Lab Completion Checklist

- [x] Setup and run application
- [x] Demonstrate single point of failure
- [x] Fix intentional crash bug
- [x] Perform load testing (Locust)
- [x] Optimize 3 routes (checkout, events, my-events)
- [x] Document performance improvements
- [x] Submit 9 screenshots (SS1-SS9)

---

## 🎓 Conclusion

**Advantages of Monoliths:** Simple development, easy deployment, good for small apps  
**Disadvantages:** Single point of failure, difficult to scale, tight coupling

**Lesson Learned:** Monolithic architecture works well for small applications, but microservices are better for production systems requiring independent scaling and fault tolerance.

