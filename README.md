# -ChatGPT-App
使用ChatGPT开发的App
以下是一个使用 Python 和 pygame 库实现的简单贪吃蛇游戏。

运行步骤：

1. 确保安装了 pygame，如果没有，请运行：

pip install pygame


2. 将以下代码保存为 snake_game.py，然后运行：

python snake_game.py



代码：

import pygame
import random

# 初始化 pygame
pygame.init()

# 颜色定义
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)
BLACK = (0, 0, 0)

# 窗口大小
WIDTH, HEIGHT = 500, 500
CELL_SIZE = 20

# 方向
UP = (0, -1)
DOWN = (0, 1)
LEFT = (-1, 0)
RIGHT = (1, 0)

# 初始化窗口
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("贪吃蛇")

clock = pygame.time.Clock()

def draw_snake(snake):
    for segment in snake:
        pygame.draw.rect(screen, GREEN, (segment[0], segment[1], CELL_SIZE, CELL_SIZE))

def draw_food(food):
    pygame.draw.rect(screen, RED, (food[0], food[1], CELL_SIZE, CELL_SIZE))

def main():
    snake = [(WIDTH//2, HEIGHT//2)]
    direction = RIGHT
    food = (random.randrange(0, WIDTH, CELL_SIZE), random.randrange(0, HEIGHT, CELL_SIZE))
    
    running = True
    while running:
        screen.fill(BLACK)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP and direction != DOWN:
                    direction = UP
                elif event.key == pygame.K_DOWN and direction != UP:
                    direction = DOWN
                elif event.key == pygame.K_LEFT and direction != RIGHT:
                    direction = LEFT
                elif event.key == pygame.K_RIGHT and direction != LEFT:
                    direction = RIGHT
        
        # 计算蛇的新位置
        head_x, head_y = snake[0]
        new_head = (head_x + direction[0] * CELL_SIZE, head_y + direction[1] * CELL_SIZE)

        # 碰撞检测
        if (new_head in snake or
            new_head[0] < 0 or new_head[0] >= WIDTH or
            new_head[1] < 0 or new_head[1] >= HEIGHT):
            running = False  # 游戏结束
        
        # 添加新的头部
        snake.insert(0, new_head)
        
        # 吃食物
        if new_head == food:
            food = (random.randrange(0, WIDTH, CELL_SIZE), random.randrange(0, HEIGHT, CELL_SIZE))
        else:
            snake.pop()  # 移除尾部

        # 画出蛇和食物
        draw_snake(snake)
        draw_food(food)
        
        pygame.display.flip()
        clock.tick(10)  # 控制游戏速度

    pygame.quit()

if __name__ == "__main__":
    main()

说明：

使用 pygame 绘制游戏界面，蛇和食物分别用绿色和红色的方块表示。

方向控制：

↑ (上)

↓ (下)

← (左)

→ (右)


撞墙或碰到自己时游戏结束。

每次吃到食物，蛇的长度增加。
