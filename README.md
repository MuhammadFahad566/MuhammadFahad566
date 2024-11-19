import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 400
SCREEN_HEIGHT = 600
SCREEN = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Flappy Bird")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
BLUE = (135, 206, 250)

# Bird settings
BIRD_WIDTH = 30
BIRD_HEIGHT = 30
bird_x = 50
bird_y = 300
bird_y_change = 0
gravity = 0.5
flap_strength = -10

# Pipe settings
PIPE_WIDTH = 60
PIPE_GAP = 150
pipe_x = SCREEN_WIDTH
pipe_height = random.randint(100, 400)
pipe_speed = 4

# Score
score = 0
font = pygame.font.Font(None, 36)

# Game loop
clock = pygame.time.Clock()
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bird_y_change = flap_strength

    # Bird movement
    bird_y_change += gravity
    bird_y += bird_y_change

    # Pipe movement
    pipe_x -= pipe_speed
    if pipe_x + PIPE_WIDTH < 0:
        pipe_x = SCREEN_WIDTH
        pipe_height = random.randint(100, 400)
        score += 1

    # Collision detection
    if bird_y < 0 or bird_y + BIRD_HEIGHT > SCREEN_HEIGHT:
        running = False

    if (
        bird_x + BIRD_WIDTH > pipe_x
        and bird_x < pipe_x + PIPE_WIDTH
        and not (pipe_height < bird_y < pipe_height + PIPE_GAP)
    ):
        running = False

    # Drawing
    SCREEN.fill(BLUE)
    pygame.draw.rect(SCREEN, BLACK, (bird_x, bird_y, BIRD_WIDTH, BIRD_HEIGHT))
    pygame.draw.rect(SCREEN, WHITE, (pipe_x, 0, PIPE_WIDTH, pipe_height))
    pygame.draw.rect(
        SCREEN, WHITE, (pipe_x, pipe_height + PIPE_GAP, PIPE_WIDTH, SCREEN_HEIGHT)
    )

    # Display score
    score_text = font.render(f"Score: {score}", True, BLACK)
    SCREEN.blit(score_text, (10, 10))

    pygame.display.flip()
    clock.tick(30)

# Game over
print(f"Game Over! Your score: {score}")
pygame.quit()
