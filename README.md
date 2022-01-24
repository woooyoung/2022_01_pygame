# 2022_01_pygame
```python
import pygame

# 1. 초기화
pygame.init()

# 2. 옵션설정
size = [400, 900]
screen = pygame.display.set_mode(size)

title = "My_Game_py"
pygame.display.set_caption(title)

# 3. 게임 안에서의 설정
clock = pygame.time.Clock()
black = (0,0,0)

space_ship = pygame.image.load("C:/Users/Administrator/Pictures/Saved Pictures/spaceship.png").convert_alpha()
space_ship = pygame.transform.scale(space_ship,(50,70))
space_ship_size_x, space_ship_size_y = space_ship.get_size()
space_ship_x = round(size[0]/2 - space_ship_size_x/2)
space_ship_y = size[1] - space_ship_size_y - 20

k = 0


# 4. 메인 이벤트
g_status = 0
while g_status == 0:

    # 4 - 1. FPS 설정 (frame per second)
    clock.tick(2)
    
    # 4 - 2. 입력 감지 (마우스, 키보드)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            g_status = 1
            
    # 4 - 3. 시간, 입력에 따른 변화를 반영 
    k += 1
    
    # 4 - 4. 전사 작업(그리기)
    screen.fill(black)
    screen.blit(space_ship,(space_ship_x,space_ship_y))
    # 4 - 5. 업데이트
    pygame.display.flip()
    
# 5. 종료
pygame.quit()
```