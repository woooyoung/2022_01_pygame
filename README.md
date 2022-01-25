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

class Object:
    def __init__(self):
        self.x = 0
        self.y = 0
        self.move = 0
        
    def add_img(self, address):
        if address[-3:] == "png":
            self.img = pygame.image.load(address).convert_alpha()
        else:
            self.img = pygame.image.load(address)
        
        self.size_x, self.size_y = self.img.get_size()
        
    def transform_size(self,size_x,size_y):
        self.img = pygame.transform.scale(self.img,(size_x,size_y))
        self.size_x, self.size_y = self.img.get_size()
    
    def show_img(self):
        screen.blit(self.img,(self.x, self.y))
        
spaceship = Object()
spaceship.add_img("C:/Users/Administrator/Pictures/Saved Pictures/spaceship.png")
spaceship.transform_size(50,70)
spaceship.x = round(size[0]/2 - spaceship.size_x/2)
spaceship.y = size[1] - spaceship.size_y - 20
spaceship.move = 10

left_move = False
right_move = False
space_move = False
        
missile_list = []

black = (0,0,0)
white = (255,255,255)
k = 0


# 4. 메인 이벤트
g_status = 0
while g_status == 0:

    # 4 - 1. FPS 설정 (frame per second)
    clock.tick(60)
    
    # 4 - 2. 입력 감지 (마우스, 키보드)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            g_status = 1
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                left_move = True
            elif event.key == pygame.K_RIGHT:
                right_move = True
            elif event.key == pygame.K_SPACE:
                space_move = True
                k = 0
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT:
                left_move = False
            elif event.key == pygame.K_RIGHT:
                right_move = False
            elif event.key == pygame.K_SPACE:
                space_move = False
#         print(event)
    # 4 - 3. 입력,시간에 따른 변화를 반영 
    if left_move == True:
        spaceship.x -= spaceship.move
        if spaceship.x <= 0:
            spaceship.x = 0
    elif right_move == True:
        spaceship.x += spaceship.move
        if spaceship.x >= size[0] - spaceship.size_x:
            spaceship.x = size[0] - spaceship.size_x
    
    
    if space_move == True and k % 6 == 0:
        missile = Object()
        missile.add_img("C:/Users/Administrator/Pictures/Saved Pictures/missile.png")
        missile.transform_size(10,20)
        missile.x = round(spaceship.x + spaceship.size_x/2 - missile.size_x/2)
        missile.y = spaceship.y -  missile.size_y - 10
        missile.move = 15
        missile_list.append(missile)
        
    k += 1
    
    
    delete_list = []
    for i in range(len(missile_list)):
        m = missile_list[i]
        m.y -= m.move
        if m.y <= -m.size_y:
            delete_list.append(i)
            
    delete_list.reverse()  
    for d in delete_list:
        del missile_list[d]
        
#     try:
#     delete_list.reverse() 
#     for d in delete_list:
#         del missile_list[d]
#     except:
#         pass
        
    # 4 - 4. 전사 작업(그리기)
    screen.fill(black)
    spaceship.show_img()
    for missile in missile_list:
        missile.show_img()
    # 4 - 5. 업데이트
    pygame.display.flip()
    
# 5. 종료
pygame.quit()
```