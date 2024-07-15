# -6import math
import sqlite3
from datetime import datetime

# Calculate grains of sand on Earth
def calculate_grains_of_sand(average_grain_diameter_mm=0.5, earth_surface_area_km2=510100000):
    earth_surface_area_mm2 = earth_surface_area_km2 * 1e12  # Convert from km² to mm²
    average_grain_volume_mm3 = (4/3) * math.pi * (average_grain_diameter_mm / 2)**3
    grains_per_square_mm = 1 / average_grain_volume_mm3
    total_grains = grains_per_square_mm * earth_surface_area_mm2
    return total_grains

# Example usage of the function to calculate grains of sand
grains_of_sand = calculate_grains_of_sand()
print(f"Estimated number of grains of sand on Earth: {grains_of_sand:.2e}")

# Create database for cryptocurrency events
def create_database(db_name='crypto_events.db'):
    conn = sqlite3.connect(db_name)
    cursor = conn.cursor()
    cursor.execute('''
    CREATE TABLE IF NOT EXISTS events (
        id INTEGER PRIMARY KEY,
        date TEXT,
        event TEXT,
        description TEXT
    )
    ''')
    conn.commit()
    conn.close()

# Add an event to the database
def add_event(date, event, description, db_name='crypto_events.db'):
    conn = sqlite3.connect(db_name)
    cursor = conn.cursor()
    cursor.execute('''
    INSERT INTO events (date, event, description) VALUES (?, ?, ?)
    ''', (date, event, description))
    conn.commit()
    conn.close()

# Get all events from the database
def get_all_events(db_name='crypto_events.db'):
    conn = sqlite3.connect(db_name)
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM events')
    events = cursor.fetchall()
    conn.close()
    return events

# Example usage of database functions
create_database()
add_event(datetime.now().strftime("%Y-%m-%d %H:%M:%S"), 'Bitcoin Halving', 'The reward for mining new Bitcoin blocks is halved.')
events = get_all_events()
for event in events:
    print(event)
