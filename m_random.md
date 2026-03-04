# Python Random Module - ULTIMATE COMPLETE GUIDE

---

## WHAT IS RANDOM?

The `random` module generates pseudo-random numbers and performs random operations. It uses the **Mersenne Twister** algorithm (fast, threadsafe, 53-bit precision).

**Import:**
```python
import random
```

**⚠️ WARNING:** NOT for cryptographic/security purposes! Use `secrets` module instead.

---

## PART 1: BOOKKEEPING FUNCTIONS (State Control)

### random.seed() - Set Random Seed

```python
import random

# Seed with None (default) - uses system time
random.seed()

# Seed with int - reproducible results
random.seed(42)
print(random.random())  # 0.6394267984578837
print(random.random())  # 0.025891946446564603

# Reset with same seed
random.seed(42)
print(random.random())  # 0.6394267984578837 (same!)
print(random.random())  # 0.025891946446564603 (same!)

# Seed with string/bytes
random.seed("hello")
print(random.random())  # Reproducible from this string

# Seed with float
random.seed(3.14)
print(random.random())  # Reproducible from this float
```

---

### random.getstate() - Get Generator State

```python
import random

random.seed(42)
state = random.getstate()

# Get some random numbers
print(random.random())  # 0.6394267984578837
print(random.random())  # 0.025891946446564603

# State captures the sequence at a point in time
```

---

### random.setstate() - Restore Generator State

```python
import random

# First sequence
random.seed(42)
state = random.getstate()
print(random.random())  # 0.6394267984578837
print(random.random())  # 0.025891946446564603

# Get more numbers
print(random.random())  # 0.27775239541316198

# Restore to saved state
random.setstate(state)
print(random.random())  # 0.025891946446564603 (continues from saved state)
```

---

## PART 2: FUNCTIONS FOR BYTES

### random.randbytes() - Generate Random Bytes

```python
import random

# Generate 10 random bytes
random_bytes = random.randbytes(10)
print(random_bytes)  # b'\x1f\x9e\x8c\x92...'
print(len(random_bytes))  # 10

# Generate 0 bytes
random_bytes = random.randbytes(0)
print(random_bytes)  # b''

# Use case: generate random filenames
import binascii
random_id = binascii.hexlify(random.randbytes(8)).decode()
print(random_id)  # 'a1b2c3d4e5f6g7h8'

# ⚠️ NOT for security! Use secrets.token_bytes() instead
```

---

## PART 3: FUNCTIONS FOR INTEGERS

### random.random() - Base Function (Float 0.0 to 1.0)

```python
import random

# Basic random float
print(random.random())  # 0.37444887175646646
print(random.random())  # 0.5668992893923206
print(random.random())  # 0.09693054761903847

# Returns: 0.0 <= X < 1.0 (1.0 is NOT included)
```

---

### random.randrange() - Random Integer from Range

```python
import random

# Single argument: 0 to stop (exclusive)
print(random.randrange(10))      # 0-9
print(random.randrange(10))      # 0-9

# Two arguments: start to stop (exclusive)
print(random.randrange(1, 10))   # 1-9
print(random.randrange(1, 100))  # 1-99

# With step
print(random.randrange(0, 10, 2))    # 0, 2, 4, 6, 8
print(random.randrange(1, 10, 2))    # 1, 3, 5, 7, 9

# Large ranges (optimized)
print(random.randrange(1_000_000_000))  # Works efficiently

# Error if start > stop
# random.randrange(10, 1)  # ValueError!
```

---

### random.randint() - Inclusive Integer Range

```python
import random

# Both endpoints INCLUSIVE
print(random.randint(1, 10))    # 1-10 (including 10!)
print(random.randint(1, 100))   # 1-100

# Use case: dice roll
print(random.randint(1, 6))  # 1-6
print(random.randint(1, 6))  # 1-6

# Use case: coin flip (0=heads, 1=tails)
print(random.randint(0, 1))  # 0 or 1

# Alias for randrange(a, b+1)
print(random.randint(1, 10) == random.randrange(1, 11))  # True
```

---

### random.getrandbits() - Random Bits

```python
import random

# Get integer with k random bits
print(random.getrandbits(1))     # 0 or 1
print(random.getrandbits(8))     # 0-255
print(random.getrandbits(16))    # 0-65535
print(random.getrandbits(32))    # Large number

# Useful for random bytes
bits = random.getrandbits(100)
print(bits)  # Random 100-bit integer

# Use case: get bytes as integer
bytes_as_int = random.getrandbits(32)
print(bytes_as_int)  # 4294967295 or less
```

---

## PART 4: FUNCTIONS FOR SEQUENCES

### random.choice() - Pick One Random Element

```python
import random

# From list
colors = ['red', 'blue', 'green', 'yellow']
print(random.choice(colors))  # 'green' (random)
print(random.choice(colors))  # 'red' (random)

# From string
print(random.choice('abc'))  # 'b'
print(random.choice('hello'))  # 'l'

# From tuple
print(random.choice((1, 2, 3)))  # 2

# From range
print(random.choice(range(10)))  # 7

# Use case: random name
names = ['Alice', 'Bob', 'Charlie']
print(f"Winner: {random.choice(names)}")  # Winner: Charlie

# Error if empty sequence
# random.choice([])  # IndexError!
```

---

### random.choices() - Pick k Elements WITH Replacement

```python
import random

# Basic: pick 5 items with replacement
colors = ['red', 'blue', 'green']
print(random.choices(colors, k=5))  # ['blue', 'red', 'red', 'green', 'blue']

# With equal probability
print(random.choices([1, 2, 3], k=10))  # [3, 1, 2, 1, 3, 2, 2, 1, 3, 2]

# With weights (relative probabilities)
# red: 40%, blue: 40%, green: 20%
outcomes = random.choices(colors, weights=[40, 40, 20], k=100)
print(outcomes.count('red'), outcomes.count('blue'), outcomes.count('green'))
# ~40, ~40, ~20

# With cumulative weights
outcomes = random.choices(colors, cum_weights=[40, 80, 100], k=100)

# Use case: biased coin (70% heads, 30% tails)
flips = random.choices(['heads', 'tails'], weights=[70, 30], k=100)
print(flips.count('heads'))  # ~70

# Use case: load balancing with server weights
servers = ['server1', 'server2', 'server3']
weights = [50, 30, 20]  # server1 gets 50% of traffic
requests = random.choices(servers, weights=weights, k=1000)

# Use case: weighted lottery
prizes = ['$100', '$10', '$1']
weights = [1, 10, 100]  # rare $100, common $1
print(random.choices(prizes, weights=weights, k=1))
```

---

### random.shuffle() - Shuffle List In Place

```python
import random

# Shuffle modifies list in place
deck = ['A', 'K', 'Q', 'J', '10']
random.shuffle(deck)
print(deck)  # ['Q', '10', 'A', 'J', 'K'] (shuffled!)

# Try again
random.shuffle(deck)
print(deck)  # Different order each time

# Use case: shuffle player order in game
players = ['Alice', 'Bob', 'Charlie', 'Diana']
random.shuffle(players)
print(players)  # Random order

# Use case: shuffle deck of cards
import string
deck = [rank + suit for rank in '23456789TJQKA' 
                      for suit in '♠♣♥♦']
random.shuffle(deck)
print(deck[:5])  # First 5 cards dealt

# Note: does NOT return anything
result = random.shuffle([1, 2, 3])
print(result)  # None!

# To shuffle immutable sequences, use sample()
original = (1, 2, 3, 4, 5)
shuffled_list = random.sample(original, k=len(original))
print(shuffled_list)  # [4, 1, 3, 5, 2]
```

---

### random.sample() - Pick k Unique Elements WITHOUT Replacement

```python
import random

# Pick 3 unique items from list
colors = ['red', 'blue', 'green', 'yellow', 'purple']
print(random.sample(colors, k=3))  # ['green', 'red', 'yellow']

# Can't pick more than population size
# random.sample([1, 2, 3], k=5)  # ValueError!

# Works with any sequence
print(random.sample(range(10), k=5))  # [3, 7, 1, 9, 4]
print(random.sample('hello', k=3))    # ['e', 'h', 'o']

# With counts (repeated elements)
colors_repeated = ['red', 'blue']
result = random.sample(colors_repeated, counts=[4, 2], k=5)
print(result)  # 5 items from [red, red, red, red, blue, blue]

# Use case: raffle/lottery (pick unique winners)
participants = list(range(1, 101))  # 100 participants
winners = random.sample(participants, k=3)
print(f"1st place: {winners[0]}, 2nd: {winners[1]}, 3rd: {winners[2]}")

# Use case: random team selection (no duplicates)
players = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve']
team1 = random.sample(players, k=3)
print(team1)  # 3 different players

# Use case: efficient sampling from large range
large_sample = random.sample(range(10_000_000), k=100)  # Very fast!

# Use case: shuffle (equivalent to sample with k=len)
shuffled = random.sample(colors, k=len(colors))
print(shuffled)  # Entire list shuffled
```

---

## PART 5: DISCRETE DISTRIBUTIONS

### random.binomialvariate() - Binomial Distribution

```python
import random

# Number of successes in n trials with probability p
# Equivalent to: sum(random() < p for i in range(n))

# Flip coin 10 times, count heads (p=0.5)
heads = random.binomialvariate(n=10, p=0.5)
print(heads)  # 0-10 (usually 4-6)

# Test study: 100 people, 75% pass rate
passes = random.binomialvariate(n=100, p=0.75)
print(passes)  # Usually ~75

# Manufacturing: 1000 items, 1% defect rate
defects = random.binomialvariate(n=1000, p=0.01)
print(defects)  # Usually ~10

# Use case: simulate multiple coin flips
flips = [random.binomialvariate(n=1, p=0.5) for _ in range(10)]
print(flips)  # [0, 1, 1, 0, 1, ...] = [T, H, H, T, H, ...]

# Use case: A/B testing simulation
conversions = random.binomialvariate(n=1000, p=0.05)
print(f"Conversions: {conversions}/1000")

# Parameters
# n: number of trials (must be >= 0)
# p: probability of success (0.0 <= p <= 1.0)
# returns: int between 0 and n
```

---

## PART 6: REAL-VALUED DISTRIBUTIONS

### random.uniform() - Uniform Distribution

```python
import random

# Random float in range [a, b]
print(random.uniform(0, 1))      # 0.0 to 1.0
print(random.uniform(1, 10))     # 1.0 to 10.0
print(random.uniform(-5, 5))     # -5.0 to 5.0

# Works in reverse too
print(random.uniform(10, 1))     # 1.0 to 10.0 (same)

# Use case: temperature simulation
temp = random.uniform(20, 30)  # 20-30°C
print(f"Temperature: {temp:.1f}°C")

# Use case: random delay (0-5 seconds)
delay = random.uniform(0, 5)
import time
# time.sleep(delay)

# Use case: random color (RGB)
r = random.uniform(0, 255)
g = random.uniform(0, 255)
b = random.uniform(0, 255)
print(f"RGB({r:.0f}, {g:.0f}, {b:.0f})")

# Use case: Monte Carlo simulation
def estimate_pi(samples=10000):
    inside = 0
    for _ in range(samples):
        x = random.uniform(-1, 1)
        y = random.uniform(-1, 1)
        if x**2 + y**2 <= 1:
            inside += 1
    return 4 * inside / samples

print(f"Estimated π: {estimate_pi()}")
```

---

### random.triangular() - Triangular Distribution

```python
import random

# Triangular distribution between low and high
# with peak at mode
print(random.triangular(0, 10))      # 0-10, peak at midpoint (5)
print(random.triangular(0, 10, 7))   # 0-10, peak at 7

# Use case: project duration estimate
# Optimistic: 2 days, Pessimistic: 10 days, Most likely: 5 days
duration = random.triangular(2, 10, 5)
print(f"Project duration: {duration:.1f} days")

# Use case: stock price movement
price = random.triangular(95, 105, 100)  # Around $100
print(f"Stock price: ${price:.2f}")

# Weighted toward mode
results = [random.triangular(0, 10, 7) for _ in range(1000)]
print(f"Average: {sum(results)/len(results):.1f}")  # Near 7
```

---

### random.betavariate() - Beta Distribution

```python
import random

# Beta distribution: values between 0 and 1
# alpha, beta > 0
print(random.betavariate(1, 1))   # Uniform [0, 1]
print(random.betavariate(2, 2))   # Symmetric around 0.5
print(random.betavariate(5, 1))   # Skewed toward 1
print(random.betavariate(1, 5))   # Skewed toward 0

# Use case: success rate simulation
success_rate = random.betavariate(alpha=8, beta=2)
print(f"Success rate: {success_rate:.2%}")  # Likely high

# Use case: user satisfaction (0-1 scale)
satisfaction = random.betavariate(2, 2)
print(f"Satisfaction: {satisfaction:.2f} (0-1 scale)")

# Use case: probability of probability (Bayesian)
# Prior belief about success probability
prob = random.betavariate(alpha=10, beta=10)
print(f"Probability estimate: {prob:.2%}")
```

---

### random.expovariate() - Exponential Distribution

```python
import random

# Exponential distribution
# lambd = 1 / mean
# Good for waiting times between events

# Average 5 seconds between arrivals
arrival_time = random.expovariate(1/5)
print(f"Next arrival in: {arrival_time:.2f} seconds")

# Average 2 minutes between emails
email_delay = random.expovariate(1/2)  # minutes
print(f"Next email in: {email_delay:.2f} minutes")

# Use case: simulate server request arrivals
for arrival in range(5):
    time = sum(random.expovariate(1/3) for _ in range(arrival+1))
    print(f"Request {arrival+1} arrives at {time:.2f}s")

# Use case: radioactive decay
halflife = random.expovariate(1)  # Exponential decay
print(f"Decay time: {halflife:.2f}")

# Use case: customer service queue simulation
def simulate_queue(num_arrivals=10):
    arrival_time = 0
    for i in range(num_arrivals):
        arrival_time += random.expovariate(1/2)  # 2-min average
        print(f"Customer {i+1} arrives at {arrival_time:.1f} min")

simulate_queue(5)
```

---

### random.gauss() - Gaussian (Normal) Distribution

```python
import random

# Normal distribution with mean μ and std dev σ
# Default: mean=0, std dev=1

print(random.gauss())             # Standard normal
print(random.gauss(100, 15))      # Mean 100, std dev 15

# Use case: IQ scores (mean=100, std=15)
iq = random.gauss(100, 15)
print(f"IQ Score: {iq:.0f}")

# Use case: test scores (mean=75, std=10)
score = random.gauss(75, 10)
print(f"Test Score: {score:.1f}")

# Use case: height simulation (mean=175cm, std=10cm)
height = random.gauss(175, 10)
print(f"Height: {height:.1f} cm")

# Use case: stock price movement (Brownian motion)
price = 100
for day in range(10):
    daily_return = random.gauss(0.001, 0.02)  # 0.1% mean, 2% volatility
    price *= (1 + daily_return)
    print(f"Day {day+1}: ${price:.2f}")

# Use case: measurement error
true_value = 50
measured = true_value + random.gauss(0, 2)  # ±2 error
print(f"Measured: {measured:.2f}")

# Faster than normalvariate() but NOT thread-safe
```

---

### random.normalvariate() - Normal Distribution (Thread-Safe)

```python
import random

# Normal distribution (same as gauss, but thread-safe)
# Slower than gauss()

print(random.normalvariate())           # Standard normal
print(random.normalvariate(100, 15))    # Mean 100, std 15

# Use case: same as gauss but in multithreaded environment
value = random.normalvariate(0, 1)
print(value)

# Thread-safe alternative to gauss()
# Use if multiple threads call simultaneously
```

---

### random.lognormvariate() - Log-Normal Distribution

```python
import random

# Log-normal: ln(X) is normally distributed
# Useful for skewed distributions (wealth, income, etc.)

print(random.lognormvariate())          # Default
print(random.lognormvariate(0, 1))      # Mean 0, std 1 (in log space)

# Use case: income distribution (skewed right)
income = random.lognormvariate(10, 0.5)
print(f"Annual Income: ${income:,.0f}")

# Use case: city population (skewed right)
population = random.lognormvariate(10, 1)
print(f"City Population: {population:,.0f}")

# Use case: file sizes
file_size = random.lognormvariate(10, 1)
print(f"File Size: {file_size:.0f} bytes")

# mu and sigma are for the ln(X), not X itself
```

---

### random.gammavariate() - Gamma Distribution

```python
import random

# Gamma distribution with shape α and scale β
# Both must be > 0

print(random.gammavariate(1, 1))    # Exponential
print(random.gammavariate(2, 2))    # Shape and scale
print(random.gammavariate(9, 2))    # More concentrated

# Use case: waiting time until n events
# Shape parameter k = number of events
# Scale parameter θ = mean time between events
wait_time = random.gammavariate(alpha=5, beta=2)
print(f"Wait for 5 events: {wait_time:.2f} time units")

# Use case: rainfall amount
rainfall = random.gammavariate(alpha=3, beta=2)
print(f"Rainfall: {rainfall:.2f} cm")

# Use case: battery life
battery_hours = random.gammavariate(alpha=4, beta=10)
print(f"Battery life: {battery_hours:.1f} hours")
```

---

### random.vonmisesvariate() - Von Mises Distribution

```python
import random
import math

# Distribution of angles
# μ: mean angle (0 to 2π radians)
# κ: concentration (0 = uniform, high = concentrated)

# Angle around 0 (north)
angle = random.vonmisesvariate(mu=0, kappa=2)
print(f"Angle: {angle:.2f} radians")

# Angle around π/2 (east), concentrated
angle = random.vonmisesvariate(mu=math.pi/2, kappa=10)
print(f"Angle (concentrated): {angle:.2f} radians")

# Uniform angle (κ=0)
angle = random.vonmisesvariate(mu=0, kappa=0)
print(f"Random angle: {angle:.2f} radians")

# Use case: compass heading with measurement error
true_heading = 45 * math.pi / 180  # 45 degrees
measured = random.vonmisesvariate(true_heading, kappa=5)
print(f"Measured heading: {measured * 180 / math.pi:.1f}°")

# Use case: wind direction around prevailing wind
prevailing_wind = 270 * math.pi / 180  # West
current_direction = random.vonmisesvariate(prevailing_wind, kappa=3)
print(f"Wind direction: {current_direction * 180 / math.pi:.1f}°")
```

---

### random.paretovariate() - Pareto Distribution

```python
import random

# Pareto distribution (power law)
# α: shape parameter (>0)

print(random.paretovariate(1))    # Flatter
print(random.paretovariate(2))    # More concentrated
print(random.paretovariate(5))    # Very skewed

# Use case: wealth distribution (80/20 rule)
wealth = random.paretovariate(alpha=1.5)
print(f"Wealth: ${wealth * 1000000:.0f}")

# Use case: city population (power law)
population = 1000000 * random.paretovariate(1)
print(f"City size: {population:.0f}")

# Use case: webpage hits (few pages get most traffic)
hits = random.paretovariate(1.5)
print(f"Page hits: {hits * 1000:.0f}")
```

---

### random.weibullvariate() - Weibull Distribution

```python
import random

# Weibull distribution
# α: scale parameter (>0)
# β: shape parameter (>0)

print(random.weibullvariate(1, 1))    # Exponential
print(random.weibullvariate(1, 2))    # Peaked
print(random.weibullvariate(2, 5))    # Different scale and shape

# Use case: product lifetime
lifetime = random.weibullvariate(alpha=100, beta=2)  # in hours
print(f"Component lifetime: {lifetime:.1f} hours")

# Use case: wind speed
wind_speed = random.weibullvariate(alpha=10, beta=2)
print(f"Wind speed: {wind_speed:.1f} m/s")

# Use case: reliability engineering
failure_time = random.weibullvariate(alpha=1000, beta=3)
print(f"Failure at: {failure_time:.1f} hours")
```

---

## PART 7: CUSTOM GENERATORS (Random Class)

### Using random.Random Class

```python
import random

# Create independent generators (don't share state)
gen1 = random.Random(42)
gen2 = random.Random(42)

# Same seed = same sequence
print(gen1.random())  # 0.6394267984578837
print(gen2.random())  # 0.6394267984578837 (identical!)

# Use case: separate generators for different threads
gen_thread1 = random.Random(1)
gen_thread2 = random.Random(2)

# Use case: reproducible but independent streams
for i in range(3):
    rand_gen = random.Random(i)
    print(f"Stream {i}: {rand_gen.randint(1, 100)}")

# All methods available on Random instances
r = random.Random()
print(r.choice([1, 2, 3]))
print(r.randint(1, 100))
print(r.gauss(0, 1))
```

---

### Using SystemRandom (OS Randomness)

```python
import random

# Uses os.urandom() - CRYPTOGRAPHICALLY SECURE
sr = random.SystemRandom()

# Slower but better for security
print(sr.random())      # From OS
print(sr.randint(1, 100))  # From OS

# Use case: generate tokens/secrets
token = ''.join(sr.choice('0123456789abcdef') for _ in range(32))
print(token)

# Note: seed() has no effect
sr.seed(42)  # Ignored
print(sr.random())  # Still random from OS

# Still not suitable for cryptography - use secrets module instead!
```

---

## PART 8: PRACTICAL EXAMPLES (35+)

### Example 1: Random Password Generator

```python
import random
import string

def generate_password(length=12):
    chars = string.ascii_letters + string.digits + '!@#$%^&*'
    return ''.join(random.choice(chars) for _ in range(length))

print(generate_password())       # a7k@mL2xP#qR
print(generate_password(20))     # Longer password
```

---

### Example 2: Shuffle Playlist

```python
import random

playlist = ['song1.mp3', 'song2.mp3', 'song3.mp3', 'song4.mp3']
random.shuffle(playlist)
print(playlist)  # Randomized order
```

---

### Example 3: Random Team Assignment

```python
import random

players = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank']
team1 = random.sample(players, k=3)
team2 = [p for p in players if p not in team1]
print(f"Team 1: {team1}")
print(f"Team 2: {team2}")
```

---

### Example 4: Weighted Lottery

```python
import random

prizes = ['$1000', '$100', '$10', 'No Prize']
weights = [1, 5, 10, 84]  # Rare prize is rare
winner = random.choices(prizes, weights=weights, k=1)[0]
print(f"You won: {winner}")
```

---

### Example 5: Dice Roll Simulator

```python
import random

def roll_dice(num_dice=1, sides=6):
    return [random.randint(1, sides) for _ in range(num_dice)]

print(roll_dice())           # 1 d6
print(roll_dice(2))          # 2 d6
print(roll_dice(3, 20))      # 3 d20
```

---

### Example 6: Random Sample from Large Range

```python
import random

# Efficient sampling from large range
sample = random.sample(range(1_000_000), k=100)
print(len(sample))  # 100
print(sample[:5])   # First 5
```

---

### Example 7: Monte Carlo Pi Estimation

```python
import random
import math

def estimate_pi(samples=100000):
    inside = 0
    for _ in range(samples):
        x = random.uniform(-1, 1)
        y = random.uniform(-1, 1)
        if x**2 + y**2 <= 1:
            inside += 1
    return 4 * inside / samples

estimated_pi = estimate_pi()
print(f"Estimated π: {estimated_pi}")
print(f"Actual π: {math.pi}")
print(f"Error: {abs(estimated_pi - math.pi):.4f}")
```

---

### Example 8: Shuffle Deck and Deal

```python
import random

deck = [f"{rank}{suit}" 
        for rank in '23456789TJQKA' 
        for suit in '♠♣♥♦']

random.shuffle(deck)
hand = deck[:5]
print(f"Your hand: {hand}")
```

---

### Example 9: Random Weighted Selection

```python
import random

# Weather probabilities
weather = random.choices(
    ['Sunny', 'Cloudy', 'Rainy', 'Snowy'],
    weights=[40, 30, 20, 10],
    k=7
)
print(f"Week forecast: {weather}")
```

---

### Example 10: Reproducible Experiments

```python
import random

random.seed(42)
results1 = [random.randint(1, 100) for _ in range(5)]

random.seed(42)
results2 = [random.randint(1, 100) for _ in range(5)]

print(results1)  # Same
print(results2)  # Same
print(results1 == results2)  # True
```

---

### Example 11: Random Intervals

```python
import random
import time

def random_delay(min_sec=1, max_sec=5):
    delay = random.uniform(min_sec, max_sec)
    # time.sleep(delay)
    return delay

print(f"Wait {random_delay():.1f}s")
```

---

### Example 12: Bootstrap Confidence Interval

```python
import random
from statistics import mean

data = [10, 15, 20, 25, 30]

means = []
for _ in range(1000):
    sample = random.choices(data, k=len(data))
    means.append(mean(sample))

means.sort()
confidence_95 = (means[25], means[975])
print(f"95% confidence interval: {confidence_95}")
```

---

### Example 13: A/B Test Simulation

```python
import random

def simulate_aab_test(control_rate=0.05, test_rate=0.06, n=1000):
    control = sum(random.random() < control_rate for _ in range(n))
    test = sum(random.random() < test_rate for _ in range(n))
    return control, test

for trial in range(5):
    c, t = simulate_aab_test()
    print(f"Trial {trial+1}: Control={c}/1000, Test={t}/1000")
```

---

### Example 14: Random Name Generator

```python
import random

first_names = ['Alice', 'Bob', 'Charlie', 'Diana']
last_names = ['Smith', 'Johnson', 'Williams', 'Brown']

names = [f"{random.choice(first_names)} {random.choice(last_names)}"
         for _ in range(5)]
print(names)
```

---

### Example 15: Coin Flip

```python
import random

def coin_flip():
    return random.choice(['Heads', 'Tails'])

for _ in range(5):
    print(coin_flip())
```

---

### Example 16: True/False Quiz

```python
import random

questions = ['Q1', 'Q2', 'Q3', 'Q4', 'Q5']
random_answers = [random.choice([True, False]) for _ in questions]
print(random_answers)
```

---

### Example 17: Raffle Winner

```python
import random

tickets = list(range(1, 101))  # 100 tickets
winners = random.sample(tickets, k=3)
print(f"1st: {winners[0]}, 2nd: {winners[1]}, 3rd: {winners[2]}")
```

---

### Example 18: Random Color

```python
import random

def random_color():
    r = random.randint(0, 255)
    g = random.randint(0, 255)
    b = random.randint(0, 255)
    return f"rgb({r}, {g}, {b})"

print(random_color())  # rgb(123, 45, 67)
```

---

### Example 19: Shuffle Cards in Hand

```python
import random

hand = ['2♠', '5♥', 'K♣', 'A♦']
random.shuffle(hand)
print(hand)
```

---

### Example 20: Restaurant Rating Simulation

```python
import random

ratings = [random.uniform(1, 5) for _ in range(100)]
avg = sum(ratings) / len(ratings)
print(f"Average rating: {avg:.1f}/5.0")
```

---

### 21-35: Additional Examples

```python
# Example 21: Random timeout
timeout = random.expovariate(1/5)

# Example 22: Probability check
if random.random() < 0.3:  # 30% chance
    print("Lucky!")

# Example 23: Weighted coin
heads = random.binomialvariate(1, 0.6)

# Example 24: Gaussian noise
noise = random.gauss(0, 0.1)

# Example 25: Batch sampling
batch = random.sample(range(10000), k=64)

# Example 26: Population diversity
diversity = random.choices([1, 2, 3, 4, 5], weights=[20, 25, 30, 15, 10], k=100)

# Example 27: Service level agreement uptime
uptime = random.betavariate(80, 20)

# Example 28: Pareto principle test
hits = [random.paretovariate(1) for _ in range(100)]

# Example 29: Normal distribution histogram
heights = [random.gauss(170, 10) for _ in range(1000)]

# Example 30: Random rotation (angles)
angle = random.vonmisesvariate(0, 5)

# Example 31: Exponential backoff retry
delays = [random.expovariate(1) for _ in range(5)]

# Example 32: Stratified sampling
strata = {1: random.sample(range(100), 10),
          2: random.sample(range(100, 200), 10)}

# Example 33: Poisson-like event timing
events = [sum(random.random() for _ in range(5)) for _ in range(10)]

# Example 34: Random subset selection
subset = random.sample(range(1, 1000), k=50)

# Example 35: Weighted shuffling (not perfect but close)
weighted = sorted(enumerate(['a', 'b', 'c']), 
                   key=lambda x: random.random())
```

---

## QUICK REFERENCE

| Task | Function | Example |
|------|----------|---------|
| Random float [0, 1) | `random()` | `random()` |
| Random float [a, b] | `uniform(a, b)` | `uniform(1, 10)` |
| Random int [a, b] (inclusive) | `randint(a, b)` | `randint(1, 6)` |
| Random int from range | `randrange(stop)` | `randrange(10)` |
| Pick one element | `choice(seq)` | `choice([1,2,3])` |
| Pick k with replacement | `choices(seq, k=k)` | `choices([1,2], k=5)` |
| Pick k unique | `sample(seq, k=k)` | `sample([1,2,3,4], k=2)` |
| Shuffle in place | `shuffle(list)` | `shuffle(cards)` |
| Set seed | `seed(value)` | `seed(42)` |
| Get state | `getstate()` | `state = getstate()` |
| Restore state | `setstate(state)` | `setstate(state)` |
| Random bytes | `randbytes(n)` | `randbytes(16)` |
| Random bits | `getrandbits(k)` | `getrandbits(32)` |
| Normal distribution | `gauss(μ, σ)` | `gauss(100, 15)` |
| Exponential | `expovariate(λ)` | `expovariate(1/5)` |
| Binomial | `binomialvariate(n, p)` | `binomialvariate(10, 0.5)` |
| Triangular | `triangular(low, high, mode)` | `triangular(0, 10, 5)` |
| Beta | `betavariate(α, β)` | `betavariate(2, 2)` |
| Gamma | `gammavariate(α, β)` | `gammavariate(2, 2)` |
| Pareto | `paretovariate(α)` | `paretovariate(1.5)` |
| Weibull | `weibullvariate(α, β)` | `weibullvariate(1, 2)` |
| Von Mises | `vonmisesvariate(μ, κ)` | `vonmisesvariate(0, 2)` |

---

## COMMON PATTERNS

```python
# Coin flip
heads = random.randint(0, 1) == 1

# Random percentage
percentage = random.randint(0, 100)

# Random element from multiple sequences
random.choice(list1) if random.random() < 0.5 else random.choice(list2)

# Weighted probability
if random.random() < 0.7:  # 70% chance
    print("Success")

# Random sleep
import time
time.sleep(random.uniform(0.5, 2.0))

# Reproducible sequence
random.seed(42)
# ... all following random calls are reproducible

# Thread-safe independent generators
gen = random.Random(42)
value = gen.randint(1, 100)
```

---

## SUMMARY

You now know:
- ✅ Bookkeeping functions (seed, getstate, setstate)
- ✅ Byte generation (randbytes)
- ✅ Integer functions (randrange, randint, getrandbits)
- ✅ Sequence functions (choice, choices, shuffle, sample)
- ✅ Discrete distributions (binomialvariate)
- ✅ Real-valued distributions (uniform, triangular, beta, exponential, gaussian, lognormal, normal, gamma, vonmises, pareto, weibull)
- ✅ Custom generators (Random, SystemRandom)
- ✅ 35+ practical examples
- ✅ Quick reference tables

**REMEMBER:** Use `secrets` module for cryptographic purposes!
