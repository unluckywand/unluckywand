import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the display
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Terrifier's Twisted Playground")

# Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Player (Art the Clown)
player_width, player_height = 40, 60
player_x = WIDTH // 2 - player_width // 2
player_y = HEIGHT - player_height - 10
player_speed = 5
player_jump = False
player_jump_count = 10

# Enemy
enemy_width, enemy_height = 30, 30
enemy_x = random.randint(0, WIDTH - enemy_width)
enemy_y = 0
enemy_speed = 3

# Game loop
clock = pygame.time.Clock()
score = 0
game_over = False

while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    keys = pygame.key.get_pressed()
    
    # Player movement
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < WIDTH - player_width:
        player_x += player_speed
    
    # Jumping
    if not player_jump:
        if keys[pygame.K_SPACE]:
            player_jump = True
    else:
        if player_jump_count >= -10:
            player_y -= (player_jump_count * abs(player_jump_count)) * 0.5
            player_jump_count -= 1
        else:
            player_jump = False
            player_jump_count = 10

    # Enemy movement
    enemy_y += enemy_speed
    if enemy_y > HEIGHT:
        enemy_x = random.randint(0, WIDTH - enemy_width)
        enemy_y = 0
        score += 1

    # Collision detection
    if (player_x < enemy_x + enemy_width and
        player_x + player_width > enemy_x and
        player_y < enemy_y + enemy_height and
        player_y + player_height > enemy_y):
        game_over = True

    # Clear the screen
    screen.fill(BLACK)

    # Draw player
    pygame.draw.rect(screen, WHITE, (player_x, player_y, player_width, player_height))

    # Draw enemy
    pygame.draw.rect(screen, RED, (enemy_x, enemy_y, enemy_width, enemy_height))

    # Display score
    font = pygame.font.Font(None, 36)
    text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(text, (10, 10))

    # Update display
    pygame.display.flip()

    # Set the frame rate
    clock.tick(60)

# Quit the game
pygame.quit()
