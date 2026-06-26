# omega-divergence
import decimal
import time

# --- CONFIGURATION ---
# Set the exact number of decimal places you want to calculate
DECIMAL_PLACES = 1000
decimal.getcontext().prec = DECIMAL_PLACES

# Set how high you want to search for primes /default = 1000000
SEARCH_LIMIT = 1000000


def generate_primes(limit):
    """
    Generates all prime numbers up to 'limit' using the Sieve of Eratosthenes.
    This is drastically faster than checking numbers one by one.
    """
    print(f"Generating primes up to {limit}...")
    
    # Create a boolean array [True, True, ... ] 
    sieve = [True] * (limit + 1)
    primes = []
    
    for p in range(2, limit + 1):
        if sieve[p]:
            primes.append(p)
            # Cross out all multiples of this prime
            for i in range(p * p, limit + 1, p):
                sieve[i] = False
                
    print(f"Found {len(primes)} prime numbers.")
    return primes

def calculate_constant(primes):
    """
    Feeds the primes into the Volatility Engine to build the constant.
    Formula: C = 1 + Sum( 1 / ((p1*p4) - (p2*p3)) )
    """
    print(f"Igniting the engine. Calculating to {DECIMAL_PLACES} decimal places...")
    
    # Start with the baseline observer state of 1
    c = decimal.Decimal(1)
    
    # Create a sliding window of 4 primes
    for i in range(len(primes) - 3):
        p1 = decimal.Decimal(primes[i])
        p2 = decimal.Decimal(primes[i+1])
        p3 = decimal.Decimal(primes[i+2])
        p4 = decimal.Decimal(primes[i+3])
        
        # Calculate the Unimodular Shear Determinant
        determinant = (p1 * p4) - (p2 * p3)
        
        # Add the fraction to the running total
        c += decimal.Decimal(1) / determinant
        
    return c

# --- EXECUTION ---
start_time = time.time()

# 1. Get the primes
prime_list = generate_primes(SEARCH_LIMIT)

# 2. Calculate the constant
final_constant = calculate_constant(prime_list)

end_time = time.time()

# 3. Output the results
print("\n--- YOUR MATHEMATICAL CONSTANT ---")
print(final_constant)
print(f"\nCalculated in {round(end_time - start_time, 2)} seconds.")
