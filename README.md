
import pygame
import random

# Initialize Pygame
pygame.init()

# Set screen dimensions
screen_width = 400
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Catch the Falling Items")

# Define colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# Game variables
basket_width = 70
basket_height = 20
basket_x = screen_width // 2 - basket_width // 2
basket_y = screen_height - basket_height - 10
basket_speed = 5

item_width = 30
item_height = 30
item_x = random.randint(0, screen_width - item_width)
item_y = -item_height
item_speed = 4

score = 0
missed_items = 0
font = pygame.font.SysFont(None, 35)

# Create the basket
def draw_basket(x, y):
    pygame.draw.rect(screen, GREEN, (x, y, basket_width, basket_height))

# Create the falling item
def draw_item(x, y):
    pygame.draw.rect(screen, RED, (x, y, item_width, item_height))

# Display score
def show_score(score):
    score_text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(score_text, (10, 10))

# Main game loop
running = True
while running:
    screen.fill((0, 0, 0))  # Clear screen
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Get pressed keys
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and basket_x > 0:
        basket_x -= basket_speed
    if keys[pygame.K_RIGHT] and basket_x < screen_width - basket_width:
        basket_x += basket_speed

    # Move the item
    item_y += item_speed
    if item_y > screen_height:
        item_y = -item_height
        item_x = random.randint(0, screen_width - item_width)
        missed_items += 1

    # Check for collision
    if (basket_x < item_x + item_width and basket_x + basket_width > item_x and
        basket_y < item_y + item_height and basket_y + basket_height > item_y):
        score += 1
        item_y = -item_height
        item_x = random.randint(0, screen_width - item_width)

    # Draw everything
    draw_basket(basket_x, basket_y)
    draw_item(item_x, item_y)
    show_score(score)

    # Check if the game is over
    if missed_items >= 3:
        game_over_text = font.render("Game Over!", True, WHITE)
        screen.blit(game_over_text, (screen_width // 2 - 100, screen_height // 2))
        pygame.display.update()
        pygame.time.delay(2000)
        running = False

    # Update display
    pygame.display.update()
    pygame.time.Clock().tick(60)  # FPS

# Quit the game
pygame.quit()
