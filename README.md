import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
PLAYER_SIZE = 50
ENEMY_SIZE = 50
FPS = 60
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Create the screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Difficult Game")

# Clock to control the frame rate
clock = pygame.time.Clock()

# Player
player = pygame.Rect(WIDTH // 2 - PLAYER_SIZE // 2, HEIGHT - PLAYER_SIZE - 10, PLAYER_SIZE, PLAYER_SIZE)

# Enemies
enemies = []

def spawn_enemy():
    enemy = pygame.Rect(random.randint(0, WIDTH - ENEMY_SIZE), 0, ENEMY_SIZE, ENEMY_SIZE)
    enemies.append(enemy)

# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()
    player_speed = 5
    if keys[pygame.K_LEFT] and player.x - player_speed > 0:
        player.x -= player_speed
    if keys[pygame.K_RIGHT] and player.x + player_speed + PLAYER_SIZE < WIDTH:
        player.x += player_speed

    # Move enemies
    for enemy in enemies:
        enemy.y += 5
        if enemy.colliderect(player):
            pygame.quit()
            sys.exit()

    # Remove enemies that are out of the screen
    enemies = [enemy for enemy in enemies if enemy.y < HEIGHT]

    # Spawn new enemies
    if random.random() < 0.02:
        spawn_enemy()

    # Draw everything
    screen.fill(WHITE)
    pygame.draw.rect(screen, RED, player)
    for enemy in enemies:
        pygame.draw.rect(screen, RED, enemy)

    pygame.display.flip()

    # Cap the frame rate
    clock.tick(FPS)
