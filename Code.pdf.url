import random
import turtle
import pygame
import time
from collections import namedtuple

# Initialize Pygame and the mixer
pygame.init()
pygame.mixer.init()

# Load and play the music
music_file = r'C:\Users\leona\Downloads\Breaking-Dawn-–.mp3'  # Connects and syncs music with animation
pygame.mixer.music.load(music_file)
pygame.mixer.music.play()

number_of_steps = 600 # adjust the speed of the sunrise, the higher the number, the slower the sun and vice versa
number_of_stars = 500
sun_size = 500  # Increase or decrease size of sun as it rises
width = 1200
height = 800

# Create Named Tuple Classes
RangeLimits = namedtuple("RangeLimits", "start, end")
Colour = namedtuple("Colour", "red, green, blue")

sky_colour = RangeLimits(
    Colour(0 / 255, 0 / 255, 0 / 255),
    Colour(0 / 255, 191 / 255, 255 / 255),
)
star_colour = RangeLimits(
    Colour(1, 1, 1),
    sky_colour.end,
)
sun_colour = RangeLimits(
    Colour(255 / 255, 215 / 255, 0 / 255),  # Bright yellow sun color
    Colour(255 / 255, 255 / 255, 0 / 255),  # Even brighter yellow
)

sun_x_coordinate = -width / 3
sun_y_position = RangeLimits(
    -height / 2 - sun_size / 2,
    height / 2 - height / 8,
)


def calculate_colour_change(
        start: Colour,
        end: Colour,
        n_steps: int,
):
    red_step = (end.red - start.red) / n_steps
    green_step = (end.green - start.green) / n_steps
    blue_step = (end.blue - start.blue) / n_steps

    return Colour(red_step, green_step, blue_step)


def calculate_movement_change(start, end, n_steps):
    return (end - start) / n_steps


# Set up the window for the animation
# Sky
sky = turtle.Screen()
sky.setup(width, height)
sky.tracer(0)
sky.title("Eternal Sunshine")
sky.bgcolor(sky_colour.start)

# Stars
stars = turtle.Turtle()
stars.hideturtle()
stars.penup()

# Generate pairs of coordinates for the star positions
star_positions = tuple(
    (
        random.randint(-width // 2, width // 2),
        random.randint(-width // 2, width // 2),
    )
    for _ in range(number_of_stars)
)
# …and size for the stars
star_sizes = tuple(
    random.randint(2, 8) for _ in range(number_of_stars)
)


def draw_stars(colour):
    stars.clear()
    stars.color(colour)
    for position, size in zip(star_positions, star_sizes):
        stars.setposition(position)
        stars.dot(size)


# Draw the initial stars using the starting colour
draw_stars(star_colour.start)

# Sun
sun = turtle.Turtle()
sun.hideturtle()
sun.setposition(sun_x_coordinate, sun_y_position.start)


def draw_sun(sun_col, sun_height, size):
    sun.clear()
    sun.color(sun_col)
    sun.sety(sun_height)
    sun.dot(size)


# Calculate step sizes needed for colour changes
sky_colour_steps = calculate_colour_change(
    sky_colour.start, sky_colour.end, number_of_steps
)
star_colour_steps = calculate_colour_change(
    star_colour.start, star_colour.end, number_of_steps
)
sun_colour_steps = calculate_colour_change(
    sun_colour.start, sun_colour.end, number_of_steps
)

sun_movement_steps = calculate_movement_change(
    sun_y_position.start, sun_y_position.end, number_of_steps
)

####
# Start animation
current_sky_colour = sky_colour.start._asdict()
current_star_colour = star_colour.start._asdict()
current_sun_colour = sun_colour.start._asdict()

current_sun_y_position = sun_y_position.start
current_sun_size = 50  # Start with a smaller size to simulate it rising as a shining star

start_time = time.time()

for step in range(number_of_steps):
    # Sky
    current_sky_colour["red"] += sky_colour_steps.red
    current_sky_colour["green"] += sky_colour_steps.green
    current_sky_colour["blue"] += sky_colour_steps.blue
    # Change the background to use the new colour
    sky.bgcolor(current_sky_colour.values())

    # Stars
    current_star_colour["red"] += star_colour_steps.red
    current_star_colour["green"] += star_colour_steps.green
    current_star_colour["blue"] += star_colour_steps.blue

    draw_stars(current_star_colour.values())

    # Sun
    current_sun_colour["red"] += sun_colour_steps.red
    current_sun_colour["green"] += sun_colour_steps.green
    current_sun_colour["blue"] += sun_colour_steps.blue
    current_sun_y_position += sun_movement_steps

    # Increase the size of the sun as it rises
    current_sun_size += (sun_size - 50) / number_of_steps

    draw_sun(current_sun_colour.values(), current_sun_y_position, current_sun_size)

    sky.update()
    time.sleep(0.01)  # Delay to control animation speed

# Wait until the music finishes playing
while pygame.mixer.music.get_busy():
    time.sleep(1)

turtle.done()
pygame.quit()
