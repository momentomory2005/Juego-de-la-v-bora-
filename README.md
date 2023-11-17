# Juego-de-la-v-bora-
Juego de la viborita
# juego 
import pygame
import sys
import random
pygame.init()
WIDTH, HEIGHT = 640, 480
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("LA VIBORITA")
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
SNAKE_SIZE = 20
snake = [(WIDTH // 2, HEIGHT // 2)]
snake_direction = (0, 0)
food = (random.randint(0, (WIDTH - SNAKE_SIZE) // SNAKE_SIZE) * SNAKE_SIZE,
        random.randint(0, (HEIGHT - SNAKE_SIZE) // SNAKE_SIZE) * SNAKE_SIZE)
score = 0
font = pygame.font.Font(None, 36)
clock = pygame.time.Clock()
def draw_snake():
    for segment in snake:
        pygame.draw.rect(screen, GREEN, (segment[0], segment[1], SNAKE_SIZE, SNAKE_SIZE))
def move_snake():
    global food, score
    x, y = snake[0]
    x += snake_direction[0]
    y += snake_direction[1]
    new_head = (x, y)
    snake.insert(0, new_head)
    if snake[0] == food:
        score += 1  
        food = (random.randint(0, (WIDTH - SNAKE_SIZE) // SNAKE_SIZE) * SNAKE_SIZE,
                random.randint(0, (HEIGHT - SNAKE_SIZE) // SNAKE_SIZE) * SNAKE_SIZE)
    else:
        snake.pop()
def show_score():
    text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(text, (10, 10))
def reset_game():
    global snake, snake_direction, food, score
    snake = [(WIDTH // 2, HEIGHT // 2)] 
    snake_direction = (0, 0) 
    food = (random.randint(0, (WIDTH - SNAKE_SIZE) // SNAKE_SIZE) * SNAKE_SIZE,
            random.randint(0, (HEIGHT - SNAKE_SIZE) // SNAKE_SIZE) * SNAKE_SIZE)
    score = 0  
def handle_events ():
    global snake_direction
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()  
            sys.exit()  
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP and snake_direction != (0, SNAKE_SIZE):
                snake_direction = (0, -SNAKE_SIZE) 
            elif event.key == pygame.K_DOWN and snake_direction != (0, -SNAKE_SIZE):
                snake_direction = (0, SNAKE_SIZE)  
            elif event.key == pygame.K_LEFT and snake_direction != (SNAKE_SIZE, 0):
                snake_direction = (-SNAKE_SIZE, 0) 
            elif event.key == pygame.K_RIGHT and snake_direction != (-SNAKE_SIZE, 0):
                snake_direction = (SNAKE_SIZE, 0)  
while True:
    handle_events()
    move_snake()
    if snake[0][0] < 0 or snake[0][0] >= WIDTH or snake[0][1] < 0 or snake[0][1] >= HEIGHT:
        reset_game() 
    for segment in snake[1:]:
        if snake[0] == segment:
            reset_game()  
    screen.fill(BLACK)
    pygame.draw.rect(screen, WHITE, (food[0], food[1], SNAKE_SIZE, SNAKE_SIZE))
    draw_snake()
    show_score()
    pygame.display.update()
    clock.tick(10)
