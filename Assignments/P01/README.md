|                    Basic Design Principles in Software Development          |
| :------------------------------------------------------------------------: |
|
# Name: Kevin Kangethe
#
# Course: 2143-Object-Oriented-Programming, Spring 25, Griffin
#
#  Purpose:
#  The initial goal is to:
#  Implement a generic DB class (Baseclass) with CRUD operations that uses Json.
#  Extend the DB class (Subclass) to handle unique data shapes and behaviors.
#
#----------------------------------------------------


import json
import os

# Base class: Handles generic JSON database operations
class JsonDB:
    def __init__(self, filepath):
        self.filepath = filepath
        self.data = self._load_data()


    # Load data from JSON file or return an empty list if file doesn't exist
    def _load_data(self):
        if not os.path.exists(self.filepath):
            return []
        with open(self.filepath, 'r') as f:
            return json.load(f)

    # Save in-memory data back to JSON file
    def _save_data(self):
        with open(self.filepath, 'w') as f:
            json.dump(self.data, f, indent=4)

    # Create a new record and return its index
    def create(self, record):
        self.data.append(record)
        self._save_data()
        return len(self.data) - 1

    # Read all data or filter based on key-value pairs
    def read(self, filters=None):
        if not filters:
            return self.data
        return [record for record in self.data if all(record.get(k) == v for k, v in filters.items())]

    # Update a record at a specific index with new values
    def update(self, index, new_data):
        if 0 <= index < len(self.data):
            self.data[index].update(new_data)
            self._save_data()
        else:
            raise IndexError("Invalid index for update")

    # Delete a record at a given index
    def delete(self, index):
        if 0 <= index < len(self.data):
            del self.data[index]
            self._save_data()
        else:
            raise IndexError("Invalid index for delete")

    # Optional: Allow use with 'with' statements for automatic saving
    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        self._save_data()

# Subclass for handling people-specific data
class PeopleDB(JsonDB):
    # Create a person record with basic validation
    def create_person(self, person):
        if "first_name" not in person or "last_name" not in person:
            raise ValueError("Missing first or last name")
        return self.create(person)

    # Find people by first and/or last name
    def find_by_name(self, first_name=None, last_name=None):
        results = []
        for person in self.data:
            if (first_name and person.get("first_name") == first_name) or \
               (last_name and person.get("last_name") == last_name):
                results.append(person)
        return results

# Subclass for meteorite-specific queries
class MeteoriteDB(JsonDB):
    # Find meteorites within a range of years
    def find_by_year_range(self, start_year, end_year):
        return [m for m in self.data if 'year' in m and start_year <= int(m['year']) <= end_year]

    # Return top N heaviest meteorites
    def find_heaviest(self, top_n=5):
        return sorted(
            [m for m in self.data if 'mass' in m],
            key=lambda m: float(m['mass']),
            reverse=True
        )[:top_n]

# Subclass for earthquake-specific queries
class EarthquakeDB(JsonDB):
    # Filter earthquakes by minimum magnitude
    def filter_by_magnitude(self, min_mag):
        return [e for e in self.data if e.get('mag', 0) >= min_mag]

    # Filter earthquakes by place keyword
    def filter_by_place(self, keyword):
        return [e for e in self.data if keyword.lower() in e.get('place', '').lower()]

# Sample usage demonstrating the base and specialized classes
def main():
    # PEOPLE DB USAGE
    print("=== People DB ===")
    people_db = PeopleDB("people.json")
    people_db.create_person({"first_name": "Teresa", "last_name": "Nguyen", "city": "Dallas"})
    people_db.create_person({"first_name": "Alex", "last_name": "Smith", "city": "Austin"})
    results = people_db.find_by_name("Teresa")
    print("Found people:", results)

    # METEORITE DB USAGE
    print("\n=== Meteorite DB ===")
    meteor_db = MeteoriteDB("meteorites.json")
    meteor_db.create({"name": "Big Rock", "mass": "1200", "year": "1955"})
    meteor_db.create({"name": "Small Pebble", "mass": "300", "year": "1910"})
    heavy = meteor_db.find_heaviest()
    print("Heaviest meteorites:", heavy)

    # EARTHQUAKE DB USAGE
    print("\n=== Earthquake DB ===")
    quake_db = EarthquakeDB("earthquakes.json")
    quake_db.create({"place": "California", "mag": 6.2})
    quake_db.create({"place": "Nevada", "mag": 4.5})
    big_quakes = quake_db.filter_by_magnitude(5.0)
    print("Big earthquakes:", big_quakes)

if __name__ == "__main__":
    main()
 |
