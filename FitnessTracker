user_metrics = {
    "name": "",
    "age": 0,
    "gender": "",
    "weight": 0.0,
    "height": 0.0,
    "bmr": 0.0
}

fitness_goals = {
    "weekly_time_target": None,
    "weekly_calorie_target": None
}

daily_intake = {
    "water_consumed": 0
}

meal_history = []
workout_history = []

BANNER_WELCOME = r"""
  _____ _ _                         _____               _
 |  ___(_) |_ _ __   ___  ___  ___ |_   _| __ __ _  ___| | _____ _ __
 | |_  | | __| '_ \ / _ \/ __|/ __|  | || '__/ _` |/ __| |/ / _ \ '__|
 |  _| | | |_| | | |  __/\__ \\__ \  | || | | (_| | (__|   <  __/ |
 |_|   |_|\__|_| |_|\___||___/|___/  |_||_|  \__,_|\___|_|\_\___|_|
"""

BANNER_GOALS = r"""
   ____             _
  / ___| ___   __ _| |___
 | |  _ / _ \ / _` | / __|
 | |_| | (_) | (_| | \__ \
  \____|\___/ \__,_|_|___/
"""

BANNER_PROFILE = r"""
  ____             __ _ _
 |  _ \ _ __ ___  / _(_) | ___
 | |_) | '__/ _ \  |_| | |/ _ \
 |  __/| | | (_) |  _| | |  __/
 |_|   |_|  \___/|_| |_|_|\___|
"""

BANNER_REPORT = r"""
  ____                       _
 |  _ \ ___ _ __   ___  _ __| |_
 | |_) / _ \ '_ \ / _ \| '__| __|
 |  _ <  __/ |_) | (_) | |  | |_
 |_| \_\___| .__/ \___/|_|   \__|
           |_|
"""

BANNER_EXIT = r"""
  ____  _                ____         __
 / ___|| |_ __ _ _   _  / ___|  __ _ / _| ___ 
 \___ \| __/ _` | | | | \___ \ / _` | |_ / _ \
  ___) | || (_| | |_| |  ___) | (_| |  _|  __/
 |____/ \__\__,_|\__, | |____/ \__,_|_|  \___|
                 |___/
"""

def calculate_bmr(weight, height, age, gender):
    if gender.lower() in ("male", "m"):
        return (10 * weight) + (6.25 * height) - (5 * age) + 5
    else:
        return (10 * weight) + (6.25 * height) - (5 * age) - 161


def setup_onboarding():
    print(BANNER_WELCOME)
    print("=" * 50)
    print("   WELCOME! LET'S SET UP YOUR FITNESS PROFILE   ")
    print("=" * 50)

    user_metrics["name"] = input("Enter your name: ").strip()

    while True:
        try:
            user_metrics["age"] = int(input("Enter your age: "))
            if user_metrics["age"] <= 0:
                print("Age must be a positive number.")
                continue
            user_metrics["height"] = float(input("Enter your height (cm): "))
            user_metrics["weight"] = float(input("Enter your weight (kg): "))
            if user_metrics["height"] <= 0 or user_metrics["weight"] <= 0:
                print("Height and weight must be positive numbers.")
                continue

            gender = input("Enter biological sex (Male/Female): ").strip().lower()
            if gender in ("male", "m", "female", "f"):
                user_metrics["gender"] = "Male" if gender in ("male", "m") else "Female"
                break
            else:
                print("Please enter 'Male' or 'Female'.")
        except ValueError:
            print("[Input Error] Please enter valid numbers for age, height, and weight.\n")

    user_metrics["bmr"] = calculate_bmr(
        user_metrics["weight"],
        user_metrics["height"],
        user_metrics["age"],
        user_metrics["gender"]
    )

    print(f"\nProfile initialized for {user_metrics['name']}!")
    print(f"Base Daily Energy Expenditure (BMR): {user_metrics['bmr']:.1f} kcal")
    print("=" * 50 + "\n")


def menu():
    print("\n" + "=" * 38)
    print(f"  FITNESS TRACKER | User: {user_metrics['name']}")
    print("=" * 38)
    print("  [1]  Log Workout")
    print("  [2]  Quick Calorie Calculator")
    print("  [3]  View / Set Fitness Goals")
    print("  [4]  Profile, BMI & Heart Rate Zones")
    print("  [5]  Workout History")
    print("  [6]  Log a Meal / Snack")
    print("  [7]  Log Water Intake")
    print("  [8]  Progress Report")
    print("  [9]  Workout Recommendations")
    print("  [10] Exit")
    print("-" * 38)
    try:
        return int(input("  Enter a number (1-10): "))
    except ValueError:
        return 0

VALID_INTENSITIES = ("low", "medium", "high")

def get_intensity():
    """Prompt until the user enters a valid intensity, defaulting to Medium."""
    raw = input("Enter intensity (Low / Medium / High) [default: Medium]: ").strip().lower()
    if raw in VALID_INTENSITIES:
        return raw.capitalize()
    if raw == "":
        return "Medium"
    print("Unrecognised intensity — defaulting to Medium.")
    return "Medium"


def get_category_selection():
    print("  1. Cardio\n  2. Strength\n  3. HIIT\n  4. Flexibility")
    choice = input("  Select category (1-4): ").strip()
    mapping = {"1": "cardio", "2": "strength", "3": "hiit", "4": "flexibility"}
    return mapping.get(choice, "unknown")


def calculate_calories_burned(category, duration, intensity):
    category_mets = {
        "cardio": 7.0,
        "strength": 4.0,
        "hiit": 9.5,
        "flexibility": 2.5
    }
    met = category_mets.get(category.lower(), 5.0)

    if intensity.lower() == "low":
        met *= 0.8
    elif intensity.lower() == "high":
        met *= 1.3

    calories = met * 3.5 * user_metrics["weight"] / 200 * duration
    return round(calories, 1)

def workoutLog():
    print("--- Log a New Workout ---")
    category = get_category_selection()
    if category == "unknown":
        print("Invalid selection. Workout not logged.")
        return

    try:
        duration = float(input("Enter duration (minutes): "))
        if duration <= 0:
            print("Duration must be positive. Workout not logged.")
            return
    except ValueError:
        print("Invalid input. Workout not logged.")
        return

    intensity = get_intensity()
    calories = calculate_calories_burned(category, duration, intensity)

    workout_history.append({
        "type": category.capitalize(),
        "duration": duration,
        "intensity": intensity,
        "calories": calories
    })

    print(f"\n✓ Logged! Estimated {calories} kcal burned in {duration:.0f} min of {category} ({intensity}).")


def calorieCount():
    print("\n--- Quick Calorie Calculator ---")
    category = get_category_selection()
    if category == "unknown":
        print("Invalid selection.")
        return
    try:
        duration = float(input("Enter planned duration (minutes): "))
        if duration <= 0:
            print("Duration must be positive.")
            return
    except ValueError:
        print("Invalid input.")
        return

    intensity = get_intensity()
    calories = calculate_calories_burned(category, duration, intensity)
    print(f"\nEstimated burn: {calories} kcal for {duration:.0f} min of {category} at {intensity} intensity.")


def fitnessGoals():
    print(BANNER_GOALS)
    print("--- Fitness Goals ---")
    time_target = fitness_goals["weekly_time_target"]
    cal_target  = fitness_goals["weekly_calorie_target"]

    print(f"  Weekly Duration Target : {f'{time_target} mins' if time_target else 'Not Set'}")
    print(f"  Weekly Calorie Target  : {f'{cal_target} kcal' if cal_target else 'Not Set'}")

    if input("\nUpdate goals? (yes/no): ").strip().lower() in ("yes", "y"):
        try:
            t = float(input("  New weekly exercise target (minutes): "))
            c = float(input("  New weekly calorie target (kcal): "))
            if t <= 0 or c <= 0:
                print("Targets must be positive. Goals unchanged.")
                return
            fitness_goals["weekly_time_target"]    = t
            fitness_goals["weekly_calorie_target"] = c
            print("✓ Goals updated!")
        except ValueError:
            print("Invalid input. Goals unchanged.")


def viewProfileAndBmi():
    print(BANNER_PROFILE)
    print("--- User Profile & Health Metrics ---")
    print(f"  Name   : {user_metrics['name']}")
    print(f"  Age    : {user_metrics['age']} yrs")
    print(f"  Sex    : {user_metrics['gender']}")
    print(f"  Height : {user_metrics['height']} cm")
    print(f"  Weight : {user_metrics['weight']} kg")
    print(f"  BMR    : {user_metrics['bmr']:.1f} kcal/day")

    h_m = user_metrics["height"] / 100
    bmi = user_metrics["weight"] / (h_m ** 2)
    print(f"\n  BMI    : {bmi:.2f}")

    if bmi < 18.5:
        status = "Underweight"
    elif bmi < 25:
        status = "Normal weight"
    elif bmi < 30:
        status = "Overweight"
    else:
        status = "Obesity"
    print(f"  Status : {status}")

    print("\n--- Target Heart Rate Zones ---")
    max_hr = 220 - user_metrics["age"]
    print(f"  Max HR          : {max_hr} BPM")
    print(f"  Fat Burn (Low)  : {int(max_hr * 0.50)} – {int(max_hr * 0.65)} BPM")
    print(f"  Aerobic (Med)   : {int(max_hr * 0.65)} – {int(max_hr * 0.80)} BPM")
    print(f"  HIIT (High)     : {int(max_hr * 0.80)} – {int(max_hr * 0.90)} BPM")
    print("-" * 40)

    if input("\nUpdate weight or height? (yes/no): ").strip().lower() in ("yes", "y"):
        try:
            new_h = float(input("  New height (cm): "))
            new_w = float(input("  New weight (kg): "))
            if new_h <= 0 or new_w <= 0:
                print("Values must be positive. Update aborted.")
                return
            user_metrics["height"] = new_h
            user_metrics["weight"] = new_w
            user_metrics["bmr"] = calculate_bmr(new_w, new_h, user_metrics["age"], user_metrics["gender"])
            print("✓ Profile updated!")
        except ValueError:
            print("Invalid input. Update aborted.")


def workoutHistory():
    print("\n--- Workout History ---")
    if not workout_history:
        print("  No workouts recorded yet.")
        return

    header = f"{'#':<4} {'Type':<12} {'Duration':>10} {'Intensity':<12} {'Calories':>10}"
    print(header)
    print("-" * len(header))
    for i, w in enumerate(workout_history, 1):
        print(f"{i:<4} {w['type']:<12} {w['duration']:>9.0f}m {w['intensity']:<12} {w['calories']:>9.1f} kcal")


def logMeal():
    print("\n--- Log a Meal / Snack ---")
    print("  1. Breakfast\n  2. Lunch\n  3. Dinner\n  4. Snack")
    meal_mapping = {"1": "Breakfast", "2": "Lunch", "3": "Dinner", "4": "Snack"}
    meal_name = meal_mapping.get(input("  Select meal type (1-4): ").strip())

    if not meal_name:
        print("Invalid selection. Meal not logged.")
        return

    try:
        calories = float(input(f"  Calories for {meal_name} (kcal): "))
        if calories < 0:
            print("Calories cannot be negative. Meal not logged.")
            return
        meal_history.append({"name": meal_name, "calories": calories})
        print(f"✓ {meal_name} ({calories:.0f} kcal) logged.")
    except ValueError:
        print("Invalid input. Meal not logged.")


def logWater():
    print("\n--- Log Water Intake ---")
    try:
        amount = int(input("  Amount of water (mL): "))
        if amount < 0:
            print("Amount cannot be negative.")
            return
        daily_intake["water_consumed"] += amount
        total = daily_intake["water_consumed"]
        bar_filled = min(int(total / 100), 30) 
        bar = "█" * bar_filled + "░" * (30 - bar_filled)
        print(f"  Water: [{bar}] {total} mL / 2000–3000 mL target")
    except ValueError:
        print("Invalid input. Water not logged.")


def progressReport():
    print(BANNER_REPORT)
    print("--- Progress Report ---")

    total_time   = sum(w["duration"] for w in workout_history)
    total_burned = sum(w["calories"] for w in workout_history)
    total_food   = sum(m["calories"] for m in meal_history)

    time_target = fitness_goals["weekly_time_target"]
    cal_target  = fitness_goals["weekly_calorie_target"]

    print(f"  Workouts Logged : {len(workout_history)}")

    if time_target:
        pct = (total_time / time_target) * 100
        bar = "█" * min(int(pct / 5), 20) + "░" * max(20 - int(pct / 5), 0)
        print(f"  Time Active     : [{bar}] {total_time:.1f} / {time_target} mins ({pct:.1f}%)")
    else:
        print(f"  Time Active     : {total_time:.1f} mins  (no weekly goal set)")

    if cal_target:
        pct = (total_burned / cal_target) * 100
        bar = "█" * min(int(pct / 5), 20) + "░" * max(20 - int(pct / 5), 0)
        print(f"  Calories Burned : [{bar}] {total_burned:.1f} / {cal_target} kcal ({pct:.1f}%)")
    else:
        print(f"  Calories Burned : {total_burned:.1f} kcal  (no weekly goal set)")

    print("\n  Meals Today:")
    if not meal_history:
        print("    No meals logged yet.")
    else:
        for m in meal_history:
            print(f"    • {m['name']:<12} {m['calories']:.0f} kcal")

    print(f"\n  Total Food Intake : {total_food:.1f} kcal")
    print(f"  Water Consumed    : {daily_intake['water_consumed']} mL  (target: 2000–3000 mL)")
    print("-" * 40)

    if total_food == 0 and total_burned == 0:
        print(f"  BMR (base burn)   : {user_metrics['bmr']:.1f} kcal")
        print("  Log meals and workouts to see your net calorie balance!")
    else:
        total_out    = user_metrics["bmr"] + total_burned
        net_calories = total_food - total_out
        print(f"  Net Calorie Balance : {net_calories:+.1f} kcal")
        if net_calories < 0:
            print(f"  → DEFICIT of {abs(net_calories):.1f} kcal  (supports weight loss)")
        elif net_calories > 0:
            print(f"  → SURPLUS of {net_calories:.1f} kcal  (supports muscle gain)")
        else:
            print("  → Perfect caloric maintenance!")


def recommendations():
    print("\n--- Workout Recommendations ---")
    if not workout_history:
        print("  Tip: Start with a light Cardio or Flexibility session to build a baseline habit!")
        return

    cal_target   = fitness_goals["weekly_calorie_target"]
    total_burned = sum(w["calories"] for w in workout_history)

    # Count workout types to suggest variety
    types_done = {w["type"].lower() for w in workout_history}

    if cal_target is None:
        if "strength" not in types_done:
            print("  Tip: You haven't done Strength training yet — try it to build muscle!")
        elif "cardio" not in types_done:
            print("  Tip: Mix in a Cardio session for endurance and heart health.")
        else:
            print("  Tip: Great variety! Keep mixing it up to avoid plateaus.")
    elif total_burned < (cal_target * 0.5):
        print("  Tip: You're below 50% of your weekly calorie goal.")
        print("       Try a HIIT session — it burns the most calories efficiently.")
    elif total_burned >= cal_target:
        print("  Tip: Goal smashed! Focus on recovery with a light Flexibility / Yoga session.")
    else:
        remaining = cal_target - total_burned
        print(f"  Tip: You're on track! {remaining:.0f} kcal left to hit your weekly goal.")
        print("       A steady Cardio session will keep the momentum going.")
        
setup_onboarding()

while True:
    choice = menu()

    match choice:
        case 1:  workoutLog()
        case 2:  calorieCount()
        case 3:  fitnessGoals()
        case 4:  viewProfileAndBmi()
        case 5:  workoutHistory()
        case 6:  logMeal()
        case 7:  logWater()
        case 8:  progressReport()
        case 9:  recommendations()
        case 10:
            print(BANNER_EXIT)
            print(f"  Thank you for using Fitness Tracker, {user_metrics['name']}. Stay healthy!")
            break
        case _:
            print("\n[Invalid] Please choose a number between 1 and 10.")
