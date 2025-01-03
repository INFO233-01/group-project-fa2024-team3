# Group Project Production Release Area
INFO-233 Group Project – Production Release Area

This area is the production release area where you will store a copy of your final code.


import requests
import webbrowser

def display_recipe(recipe, index):
    """
    Display a single recipe with enhanced formatting.

    Args:
        recipe (dict): Recipe information.
        index (int): Recipe index in the list.
    """
    recipe_info = recipe["recipe"]
    nutrients = recipe_info.get("totalNutrients", {})
    servings = recipe_info.get("yield", 1)
    calories_per_serving = recipe_info["calories"] / servings

    print(f"Recipe {index + 1}: {recipe_info['label']}")
    print("=" * 50)
    print(f"Source: {recipe_info['source']}")
    print(f"URL: {recipe_info['url']}")
    print(f"Servings: {servings}")
    print(f"Calories per Serving: {calories_per_serving:.2f} kcal")
    print("\n--- Nutritional Information (per serving) ---")
    print(f"Protein: {nutrients.get('PROCNT', {}).get('quantity', 'N/A'):.2f} g")
    print(f"Fats: {nutrients.get('FAT', {}).get('quantity', 'N/A'):.2f} g")
    print(f"Sodium: {nutrients.get('NA', {}).get('quantity', 'N/A'):.2f} mg")
    print(f"Sugars: {nutrients.get('SUGAR', {}).get('quantity', 'N/A'):.2f} g")
    print("-" * 50)
    print()

def open_recipe_in_browser(recipes):
    """
    Ask the user which recipe to open in the browser.

    Args:
        recipes (list): List of recipes.
    """
    while True:
        try:
            choice = int(input(f"Enter the number of the recipe you want to open in the browser (1-{len(recipes)}): "))
            if 1 <= choice <= len(recipes):
                webbrowser.open_new_tab(recipes[choice - 1]["recipe"]["url"])
                print(f"Opening Recipe {choice}: {recipes[choice - 1]['recipe']['label']} in the browser...")
                break
            else:
                print(f"Invalid choice! Please choose a number between 1 and {len(recipes)}.")
        except ValueError:
            print("Invalid input! Please enter a number.")

# API URL for recipe search
url = "https://edamam-recipe-search.p.rapidapi.com/api/recipes/v2"

# User input for the food item to search
food_item = input("Enter the ingredients you would like to use in your recipe, seperated by a comma: ")

# Get the user's calorie range per serving
while True:
    try:
        min_calories_per_serving = int(input(f"Enter the minimum calories per serving for '{food_item}': "))
        max_calories_per_serving = int(input(f"Enter the maximum calories per serving for '{food_item}': "))
        if min_calories_per_serving > 0 and max_calories_per_serving > min_calories_per_serving:
            break
        else:
            print("Invalid range! Ensure the minimum is positive and less than the maximum.")
    except ValueError:
        print("Invalid input! Please enter valid numbers.")

# Query parameters for the search
querystring = {
    "q": food_item,
    "type": "public",
    "from": 0,
    "to": 50
}

# API headers with your RapidAPI key
headers = {
    "x-rapidapi-key": "e04b06695bmshcbe6fdff24bbd23p1c948ejsnea10e8079a1a",
    "x-rapidapi-host": "edamam-recipe-search.p.rapidapi.com",
    "Accept-Language": "en"
}

# Fetch data from the API
response = requests.get(url, headers=headers, params=querystring)

if response.status_code == 200:
    # Parse JSON response
    data = response.json()
    
    # Filter recipes by per-serving calorie range
    recipes = [
        recipe for recipe in data.get("hits", [])
        if recipe["recipe"]["yield"] > 0 and
        min_calories_per_serving <= (recipe["recipe"]["calories"] / recipe["recipe"]["yield"]) <= max_calories_per_serving
    ]
    
    if recipes:
        print(f"\nRecipes for '{food_item}' within the calorie range of {min_calories_per_serving} to {max_calories_per_serving} kcal per serving:\n")
        
        # Inform the user if fewer than 5 recipes are available
        if len(recipes) < 5:
            print(f"Only {len(recipes)} recipe(s) found that match your criteria.\n")
        
        # Display up to 5 recipes
        for i, recipe in enumerate(recipes[:5]):
            display_recipe(recipe, i)
        
        # Allow the user to open a recipe in the browser
        open_recipe_in_browser(recipes[:5])
    else:
        print(f"No recipes found for '{food_item}' within the calorie range of {min_calories_per_serving} to {max_calories_per_serving} kcal per serving.")
        print("Consider adjusting the calorie range for more options.")
else:
    # Print error if the request fails
    print(f"Error: {response.status_code} - {response.text}")


#Intro=””” Welcome paragraph”””

#Make the code repeat after finish

import requests
import webbrowser

# Introductory message as a multi-line string
Intro = """
==================================================
Welcome to the Recipe Finder Program!

This program helps you find recipes based on ingredients you have.
You can specify a calorie range per serving, and the program will display recipes that match.
After viewing the recipes, you can choose which one to open in your browser.

Enjoy finding your next favorite recipe!
==================================================
"""
#takes a recipe and an index as inputs. 
def display_recipe(recipe, index):
    """
    Display a single recipe with enhanced formatting.

    Args:
        recipe (dict): Recipe information.
        index (int): Recipe index in the list.
    """
    
    recipe_info = recipe["recipe"]
    nutrients = recipe_info.get("totalNutrients", {})
    servings = recipe_info.get("yield", 1)
    calories_per_serving = recipe_info["calories"] / servings
#Formats and prints the recipe in detail and in a readable way with nutritional info.
    print(f"Recipe {index + 1}: {recipe_info['label']}")
    print("=" * 50)
    print(f"Source: {recipe_info['source']}")
    print(f"URL: {recipe_info['url']}")
    print(f"Servings: {servings}")
    print(f"Calories per Serving: {calories_per_serving:.2f} kcal")
    print("\n--- Nutritional Information (per serving) ---")
    print(f"Protein: {nutrients.get('PROCNT', {}).get('quantity', 'N/A'):.2f} g")
    print(f"Fats: {nutrients.get('FAT', {}).get('quantity', 'N/A'):.2f} g")
    print(f"Sodium: {nutrients.get('NA', {}).get('quantity', 'N/A'):.2f} mg")
    print(f"Sugars: {nutrients.get('SUGAR', {}).get('quantity', 'N/A'):.2f} g")
    print("-" * 50)
    print()
#Url websearch
def open_recipe_in_browser(recipes):
    """
    Ask the user which recipe to open in the browser.

    Args:
        recipes (list): List of recipes.
    """
    while True:
        try:
            choice = int(input(f"Enter the number of the recipe you want to open in the browser (1-{len(recipes)}): "))
            if 1 <= choice <= len(recipes):
                webbrowser.open_new_tab(recipes[choice - 1]["recipe"]["url"])
                print(f"Opening Recipe {choice}: {recipes[choice - 1]['recipe']['label']} in the browser...")
                break
            else:
                print(f"Invalid choice! Please choose a number between 1 and {len(recipes)}.")
        except ValueError:
            print("Invalid input! Please enter a number.")

# Main program loop
while True:
    print(Intro)

# API URL for recipe search
    url = "https://edamam-recipe-search.p.rapidapi.com/api/recipes/v2"

# User input for the food item to search
    food_item = input("Enter the ingredients you would like to use in your recipe, separated by a comma: ")

# Get the user's calorie range per serving
    while True:
        try:
            min_calories_per_serving = int(input(f"Enter the minimum calories per serving for '{food_item}': "))
            max_calories_per_serving = int(input(f"Enter the maximum calories per serving for '{food_item}': "))
            if min_calories_per_serving >= 100 and max_calories_per_serving > min_calories_per_serving:
                break
            else:
                print("Invalid range! Ensure the minimum is positive and less than the maximum.")
        except ValueError:
            print("Invalid input! Please enter valid numbers.")

# Query parameters for the API search
    querystring = {
        "q": food_item,
        "type": "public",
        "from": 0,
        "to": 50
    }

# API headers with your RapidAPI key
    headers = {
        "x-rapidapi-key": "e04b06695bmshcbe6fdff24bbd23p1c948ejsnea10e8079a1a",
        "x-rapidapi-host": "edamam-recipe-search.p.rapidapi.com",
        "Accept-Language": "en"
    }

# Fetch data from the API
    response = requests.get(url, headers=headers, params=querystring)

    if response.status_code == 200:
        # Parse JSON response
        data = response.json()
        
# Filter recipes by per-serving calorie range
        recipes = [
            recipe for recipe in data.get("hits", [])
            if recipe["recipe"]["yield"] > 0 and
            min_calories_per_serving <= (recipe["recipe"]["calories"] / recipe["recipe"]["yield"]) <= max_calories_per_serving
        ]
        
        if recipes:
            print(f"\nRecipes for '{food_item}' within the calorie range of {min_calories_per_serving} to {max_calories_per_serving} kcal per serving:\n")
            
# Inform the user if fewer than 5 recipes are available
            if len(recipes) < 5:
                print(f"Only {len(recipes)} recipe(s) found that match your criteria.\n")
            
# Display up to 5 recipes
            for i, recipe in enumerate(recipes[:5]):
                display_recipe(recipe, i)
            
# Allow the user to open a recipe in the browser
            open_recipe_in_browser(recipes[:5])
        else:
            print(f"No recipes found for '{food_item}' within the calorie range of {min_calories_per_serving} to {max_calories_per_serving} kcal per serving.")
            print("Consider adjusting the calorie range for more options.")
    else:
        print(f"Error: {response.status_code} - {response.text}")

# Ask if the user wants to restart or quit
    restart = input("Would you like to search for another recipe? (yes/no): ").strip().lower()
    if restart != "yes":
        print("Thank you for using the Recipe Finder Program! Goodbye!")
        break
