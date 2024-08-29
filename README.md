게임 주제
슈팅 게임 (참고 예시: https://www.youtube.com/watch?v=DHgj5jhMJKg)

게임 하는 방법
게임 이동 키: A/D 또는 왼쪽/오른쪽 방향키

게임 점프 키: W 또는 SPACE BAR

총 쏘는 키: K

재장전 키: R (2초 뒤 총알 충전)

달리기 및 슈퍼 점프: Shift + A/D/왼쪽/오른쪽 방향키, Shift + w/위쪽 방향키

주의 사항 : 총탄이 다 떨어질시 발사 불가/Visual Code의 Open folder로 2024105269_lihymin_pygame1을 열어서 플레이 해주세요!

첫 시작 (import pygame)

import pygame

게임 화면 (메뉴 화면)

class MenuScreen:
    def __init__(self, Screen):
        self.Screen = Screen
        
        # 게임 화면 바꾸는 로직
    def handle_event(self, event):
        if event.type == pygame.MOUSEBUTTONUP:
            if pygame.Rect(int(ScreenWidth / 2 - start_button.get_width() / 2), int(ScreenHeight / 2 - start_button.get_height() / 2), start_button.get_width(), start_button.get_height()).collidepoint(x, y):
                return "game"

    def draw(self):
        # 메뉴 화면 그리기 로직
        Screen.blit(background, (0,0))
        Screen.blit(start_button, (int(ScreenWidth / 2 - start_button.get_width() / 2), int(ScreenHeight / 2 - start_button.get_height() / 2)))
        Screen.blit(exit_button_image, (int(ScreenWidth - exit_button_image.get_width()), int(ScreenHeight - exit_button_image.get_height())))
        Screen.blit(text5, (int(ScreenWidth / 2 - text5.get_width() / 2), int(ScreenHeight / 2 - (5/2 * text5.get_height())))) # 게임 종료

게임 화면 (게임 화면)

class GameScreen:
    def __init__(self, Screen):
        self.Screen = Screen

        # 게임 화면 바꾸는 로직
    def handle_event(self, event):
        if event.type == pygame.MOUSEBUTTONUP:
            if pygame.Rect(int(ScreenWidth / 2 - start_button.get_width() / 2), int(ScreenHeight / 2 - start_button.get_height() / 2), start_button.get_width(), start_button.get_height()).collidepoint(x, y):
                return "game"

    def draw(self):
        # 게임 화면 그리기 로직
        Screen.blit(background, (0,0))
        Screen.blit(ground.ground_image, (0,600))
        Screen.blit(exit_button_image, (int(ScreenWidth - exit_button_image.get_width()), int(ScreenHeight - exit_button_image.get_height())))
        pygame.draw.polygon(Screen, (64,64,64), [[18,30],[18,65],[195,65],[215,30]])
        Screen.blit(hp_bar, (18,10))
        pygame.draw.rect(Screen, (192,0,0), (int(ScreenWidth/2 - boss_hp_bar_image.get_width()/2 + 15), 664, int(boss_hp_bar_image.get_width() - 3),int(boss_hp_bar_image.get_height() - 5)))
        pygame.draw.rect(Screen, (0,0,0), (int(ScreenWidth/2 - boss_hp_bar_image.get_width()/2 + 15), 664, int(boss_hp_bar_image.get_width() - 3),int(boss_hp_bar_image.get_height() - 5)), 7)
        pygame.draw.polygon(Screen, (32,32,32), [[int(ScreenWidth/2 + boss_hp_bar_image.get_width()/2 + 5 + boss_damage), 670],[int(ScreenWidth/2 + boss_hp_bar_image.get_width()/2 + 5 + boss_damage), 692],[int(ScreenWidth/2 + boss_hp_bar_image.get_width()/2 + 5), 692],[int(ScreenWidth/2 + boss_hp_bar_image.get_width()/2 + 5), 670]])
        pygame.draw.circle(Screen, (224,224,224), (int(ScreenWidth/2 - boss_hp_bar_image.get_width()/2 + 10),678), 30)
        Screen.blit(boss.boss_image2, (int(ScreenWidth/2 - boss_hp_bar_image.get_width()/2 - 5),655))
        pygame.draw.circle(Screen, (0,0,0), (int(ScreenWidth/2 - boss_hp_bar_image.get_width()/2 + 10),678), 30, 5)
        pygame.draw.rect(Screen, (255,0,0), (57,20,game_player.player_hp,12))
        pygame.draw.rect(Screen, (0,0,0), (57,29,215,3))
        pygame.draw.rect(Screen, (255,255,255), (57,29,game_player.player_stamina,3))
        pygame.draw.rect(Screen, (96,96,96), (268,20,4,12))
        Screen.blit(text1, (840,20)) # 점수 (SCORE)
        Screen.blit(text4, (23,36)) # 총탄의 갯수 (BULLET COUNT)
        for i in range(bullet.count):
            Screen.blit(bullet.image_rotate,(90 + i * 12, 29))
캐릭터 움직임 및 스테미나

class Shooter(pygame.sprite.Sprite):
    # Shooter(플레이어 및 적)의 크기와 위치, 이동속도를 입력 파라메터로 가지고 변수를 만듦
    def __init__(self, scale):
        pygame.sprite.Sprite.__init__(self)
        img_main_character = pygame.image.load('image_source/soldier_idle.png')
        self.image = pygame.transform.scale(img_main_character, (int(img_main_character.get_width() * scale), int(img_main_character.get_height() * scale)))
        self.rect = self.image.get_rect()
        self.speed = 5
        self.direction = 1
        self.flip = False
        self.player_hp = 215
        self.player_stamina = 215
    
    # 플레이어의 이동 방향 및 스테미나 구현
    def player_move(self, scale, moving_left, moving_right, moving_clock):

        dx = 0

        if moving_left == False and moving_right == False:
            img_main_character = pygame.image.load('image_source/soldier_idle.png')
            self.image = pygame.transform.scale(img_main_character, (int(img_main_character.get_width() * scale), int(img_main_character.get_height() * scale)))
        if moving_left == True or moving_right == True:
            if pygame.time.get_ticks() - moving_clock < 250:
                img_main_character = pygame.image.load('image_source/soldier_walk1.png')
            if pygame.time.get_ticks() - moving_clock >= 250:
                img_main_character = pygame.image.load('image_source/soldier_walk2.png')
            self.image = pygame.transform.scale(img_main_character, (int(img_main_character.get_width() * scale), int(img_main_character.get_height() * scale)))
        if jumping:
            img_main_character = pygame.image.load('image_source/soldier_jump.png')
            self.image = pygame.transform.scale(img_main_character, (int(img_main_character.get_width() * scale), int(img_main_character.get_height() * scale)))
        if self.rect.x >= 0 and (self.rect.x + self.image.get_width()) <= 960:
            if moving_left == True:
                dx = -self.speed
                self.flip = True
                self.direction = -1
            if moving_right == True:
                dx = self.speed
                self.flip = False
                self.direction = 1
            if moving_left == True or moving_right == True:
                if running == True:
                    game_player.speed = 12
                    if game_player.player_stamina == 0:
                        game_player.speed = 5
                    if game_player.player_stamina > 0:
                        game_player.player_stamina += -4
                        if game_player.player_stamina <= 0:
                            game_player.player_stamina = 0
            if running == False:
                game_player.speed = 5
                if game_player.player_stamina < 215:
                    game_player.player_stamina += 1
                    if game_player.player_stamina >= 215:
                        game_player.player_stamina = 215

        if self.rect.x < 0:
            self.rect.x = 0
            self.flip = True
            self.direction = -1
        if (self.rect.x + self.image.get_width()) > 960:
            self.rect.x = 960 - self.image.get_width()
            self.flip = False
            self.direction = 1

        self.rect.x += dx

캐릭터 점프

    # 플레이어의 점프 구현
    def player_jump(self, dy):
        if self.rect.y == 0:
            self.dy = dy
            if running == True:
                if game_player.player_stamina >= 50:
                    self.dy = dy * 5/3
                    game_player.player_stamina -= 50
        self.dy += 0.75
        self.rect.y += self.dy
        Screen.blit(pygame.transform.flip(self.image, self.flip, False), (self.rect.x ,self.rect.y + Screen.get_size()[1] * 5/6 - self.image.get_height() + 3))
        if self.rect.y + Screen.get_size()[1] * 5/6 - self.image.get_height() + 3 >= Screen.get_size()[1] * 5/6 - self.image.get_height() + 3:
            self.rect.y = 0
        if game_player.player_hp <= 0:
            pygame.draw.rect(Screen, (0,0,0), (0,0,Screen.get_size()[0],Screen.get_size()[1]))
            Screen.blit(text6, (int(Screen.get_size()[0]/2 - text6.get_rect().size[0]/2),int(Screen.get_size()[1]/2 - text6.get_rect().size[1]/2)))

캐릭터 그리기

# 플레이어의 움직임과 점프 최종 구현
    def player_draw(self):
        Screen.blit(pygame.transform.flip(self.image, self.flip, False), (self.rect.x ,self.rect.y + Screen.get_size()[1] * 5/6  - self.image.get_height() + 3))
        if game_player.player_hp <= 0:
            pygame.draw.rect(Screen, (0,0,0), (0,0,Screen.get_size()[0],Screen.get_size()[1]))
            Screen.blit(text6, (int(Screen.get_size()[0]/2 - text6.get_rect().size[0]/2),int(Screen.get_size()[1]/2 - text6.get_rect().size[1]/2)))
        if boss_damage <= -570:
            pygame.draw.rect(Screen, (0,0,0), (0,0,Screen.get_size()[0],Screen.get_size()[1]))
            Screen.blit(text7, (int(Screen.get_size()[0]/2 - text6.get_rect().size[0]/2),int(Screen.get_size()[1]/2 - text6.get_rect().size[1]/2)))

총알 방향

class Bullet(pygame.sprite.Sprite):
    def __init__(self, scale, count):
        pygame.sprite.Sprite.__init__(self)
        img_bullet = pygame.image.load('image_source/bullet2.png')
        self.image = pygame.transform.scale(img_bullet, (int(img_bullet.get_width() * scale), int(img_bullet.get_height() * scale)))
        self.image_rotate = pygame.transform.scale(img_bullet, (int(img_bullet.get_width() * scale), int(img_bullet.get_height() * scale)))
        self.image_rotate = pygame.transform.rotate(self.image_rotate,-30)
        self.rect = self.image.get_rect()
        self.speed = 12
        self.direction = 1
        self.flip = False
        self.count = count
        self.bullet_y1 = 0
        self.bullet_y2 = 0
    # Bullet의 이동 방향 구현
    def bullet_move_direction(self, moving_left, moving_right):
        if not shooting:
            if moving_left == True:
                self.flip = True
                self.direction = -1
    
            if moving_right == True:
                self.flip = False
                self.direction = 1

        if shooting:
            if moving_left == True:
                if pygame.time.get_ticks() - bullet_start_clock <= 10:
                    self.flip = True
                    self.direction = -1
    
            if moving_right == True:
                if pygame.time.get_ticks() - bullet_start_clock <= 10:
                    self.flip = False
                    self.direction = 1

총알 재장전

def bullet_reload(self):
        if pygame.time.get_ticks() - bullet_reload_clock >= 1999:
            bullet.count = 8
총알 그리기

# Bullet의 총탄 움직임 구현
    def bullet_draw(self):
        self.image = pygame.transform.rotate(self.image_rotate, -60)
        self.moving_x = game_player.rect.x + img_main_character.get_width() / 2 - 15 + 20 * self.direction + ((pygame.time.get_ticks() - bullet_start_clock) / 0.8) * self.direction
        self.moving_y = self.bullet_y1 +(Screen.get_size()[1] * 5/6)  - self.bullet_y2 + 42
        bullet.rect.x = self.moving_x
        if pygame.time.get_ticks() - bullet_start_clock <= 25:
            self.bullet_x1 = game_player.rect.x
            self.bullet_y1 = game_player.rect.y
            self.bullet_y2 = game_player.image.get_height()
        if pygame.time.get_ticks() - bullet_start_clock <= 350:
            Screen.blit(pygame.transform.flip(self.image, self.flip, False), (bullet.moving_x, bullet.moving_y))
        if game_player.player_hp <= 0:
            pygame.draw.rect(Screen, (0,0,0), (0,0,Screen.get_size()[0],Screen.get_size()[1]))
            Screen.blit(text6, (int(Screen.get_size()[0]/2 - text6.get_rect().size[0]/2),int(Screen.get_size()[1]/2 - text6.get_rect().size[1]/2)))
        if boss_damage <= -570:
            pygame.draw.rect(Screen, (0,0,0), (0,0,Screen.get_size()[0],Screen.get_size()[1]))
            Screen.blit(text7, (int(Screen.get_size()[0]/2 - text6.get_rect().size[0]/2),int(Screen.get_size()[1]/2 - text6.get_rect().size[1]/2)))
보스 제1 패턴

class Boss(pygame.sprite.Sprite):
    def __init__(self, boss_image_width, boss_image_height):
        pygame.sprite.Sprite.__init__(self)
        self.boss = pygame.image.load('image_source/idle1.png')
        self.boss_image_width = boss_image_width 
        self.boss_image_height = boss_image_height
        self.boss_image = pygame.transform.scale(pygame.transform.flip(self.boss, True, False), (self.boss_image_width, self.boss_image_height))
        self.boss_image2 = pygame.transform.scale(pygame.transform.flip(self.boss, False, False), (int(self.boss_image_width * 1/4), int(self.boss_image_height * 1/4)))
        self.rect = self.boss_image.get_rect()
    # 보스 패턴 구현
    def boss_animation(self, boss_idle_clock):
        index1 = 1
        index2 = 1
        self.flip = True
        for index_of_index1 in range(1,4):
            if pygame.time.get_ticks() - boss_idle_clock >= 150 * index_of_index1:
                index1 += 1
        if pygame.time.get_ticks() - boss_idle_clock >= 600:
            index1 = 1
        if pygame.time.get_ticks() - boss_idle_clock2 > 1800 and pygame.time.get_ticks() - boss_idle_clock2 < 3000:
            boss.rect.x -= 12
            for index_of_index2 in range(1,10):
                if pygame.time.get_ticks() - boss_idle_clock2 >= (1800 + 100 * index_of_index2):
                    index2 += 1
        if pygame.time.get_ticks() - boss_idle_clock2 > 3000 and pygame.time.get_ticks() - boss_idle_clock2 < 4800:
            self.flip = False
        if pygame.time.get_ticks() - boss_idle_clock2 > 4800 and pygame.time.get_ticks() - boss_idle_clock2 < 6000:
            self.flip = False
            boss.rect.x += 12
            for index_of_index3 in range(1,10):
                if pygame.time.get_ticks() - boss_idle_clock2 >= (4800 + 100 * index_of_index3):
                    index2 += 1
        if (pygame.time.get_ticks() - boss_idle_clock2 <= 1800) or (pygame.time.get_ticks() - boss_idle_clock2 > 3000 and pygame.time.get_ticks() - boss_idle_clock2 < 4800):
            self.boss = pygame.image.load(f'image_source/idle{index1}.png')
        if (pygame.time.get_ticks() - boss_idle_clock2 > 1800 and pygame.time.get_ticks() - boss_idle_clock2 < 3000) or (pygame.time.get_ticks() - boss_idle_clock2 > 4800 and pygame.time.get_ticks() - boss_idle_clock2 < 6000):
            self.boss = pygame.image.load(f'image_source/Run{index2}.png')
        self.boss_image = pygame.transform.scale(pygame.transform.flip(self.boss, self.flip, False), (self.boss_image_width, self.boss_image_height))
보스 제2 패턴

class Thorn(pygame.sprite.Sprite):
    def __init__(self, type):
        pygame.sprite.Sprite.__init__(self)
        self.thorn = pygame.image.load(f'image_source/thorn{type}.png')
        self.thorn_image = pygame.transform.scale(self.thorn, (int(self.thorn.get_width() * 1/2), int(self.thorn.get_height() * 3/4)))
        self.rect = self.thorn_image.get_rect()
        self.flip = False
    # 가시 그리기
    def thorn_draw(self, boss_idle_clock3):
        index3 = 0
        if pygame.time.get_ticks() - boss_idle_clock3 >= 12000 and pygame.time.get_ticks() - boss_idle_clock3 < 15000:
            boss.rect.x = 10000
            self.flip = False
            for index_of_index4 in range(1,61):
                if pygame.time.get_ticks() - boss_idle_clock3 >= 12000 + 20 * index_of_index4:
                    index3 += thorn2.thorn_image.get_width() * 1/20
                    thorn2.rect.x = index3 - thorn2.thorn_image.get_width()
        if pygame.time.get_ticks() - boss_idle_clock3 >= 15000 and pygame.time.get_ticks() - boss_idle_clock3 < 18000:
            self.flip = True
            for index_of_index5 in range(1,61):
                if pygame.time.get_ticks() - boss_idle_clock3 >= 15000 + 25 * index_of_index5:
                    index3 -= thorn2.thorn_image.get_width() * 1/20
                    thorn2.rect.x = index3 + 950
        if pygame.time.get_ticks() - boss_idle_clock3 >= 17990:
            boss.rect.x = 840
            thorn2.rect.x = -thorn2.thorn_image.get_width() * 10
        self.thorn_image = pygame.transform.scale(self.thorn_image, (int(self.thorn.get_width() * 1/2), int(self.thorn.get_height() * 3/4)))
        Screen.blit(pygame.transform.flip(self.thorn_image, thorn2.flip, False),(thorn2.rect.x,340))
보스 그리기

 def boss_draw(self):
        Screen.blit(boss.boss_image, (boss.rect.x,420))
        if game_player.player_hp <= 0:
            pygame.draw.rect(Screen, (0,0,0), (0,0,Screen.get_size()[0],Screen.get_size()[1]))
            Screen.blit(text6, (int(Screen.get_size()[0]/2 - text6.get_rect().size[0]/2),int(Screen.get_size()[1]/2 - text6.get_rect().size[1]/2)))
        if boss_damage <= -570:
            pygame.draw.rect(Screen, (0,0,0), (0,0,Screen.get_size()[0],Screen.get_size()[1]))
            Screen.blit(text7, (int(Screen.get_size()[0]/2 - text6.get_rect().size[0]/2),int(Screen.get_size()[1]/2 - text6.get_rect().size[1]/2)))
땅,가시 그리기

class Ground(pygame.sprite.Sprite):
    def __init__(self, ground_width, ground_height):
        pygame.sprite.Sprite.__init__(self)
        self.ground = pygame.image.load('image_source/ground.png')
        self.ground_width = ground_width
        self.ground_height = ground_height
        self.ground_image = pygame.transform.scale(self.ground, (int(self.ground_width), int(self.ground_height)))
class Thorn(pygame.sprite.Sprite):
    def __init__(self, type):
        pygame.sprite.Sprite.__init__(self)
        self.thorn = pygame.image.load(f'image_source/thorn{type}.png')
        self.thorn_image = pygame.transform.scale(self.thorn, (int(self.thorn.get_width() * 1/2), int(self.thorn.get_height() * 3/4)))
        self.rect = self.thorn_image.get_rect()
        self.flip = False
    # 가시 그리기
    def thorn_draw(self, boss_idle_clock3):
        index3 = 0
        if pygame.time.get_ticks() - boss_idle_clock3 >= 12000 and pygame.time.get_ticks() - boss_idle_clock3 < 15000:
            boss.rect.x = 10000
            self.flip = False
            for index_of_index4 in range(1,61):
                if pygame.time.get_ticks() - boss_idle_clock3 >= 12000 + 20 * index_of_index4:
                    index3 += thorn2.thorn_image.get_width() * 1/20
                    thorn2.rect.x = index3 - thorn2.thorn_image.get_width()
        if pygame.time.get_ticks() - boss_idle_clock3 >= 15000 and pygame.time.get_ticks() - boss_idle_clock3 < 18000:
            self.flip = True
            for index_of_index5 in range(1,61):
                if pygame.time.get_ticks() - boss_idle_clock3 >= 15000 + 25 * index_of_index5:
                    index3 -= thorn2.thorn_image.get_width() * 1/20
                    thorn2.rect.x = index3 + 950
        if pygame.time.get_ticks() - boss_idle_clock3 >= 17990:
            boss.rect.x = 840
            thorn2.rect.x = -thorn2.thorn_image.get_width() * 10
        self.thorn_image = pygame.transform.scale(self.thorn_image, (int(self.thorn.get_width() * 1/2), int(self.thorn.get_height() * 3/4)))
        Screen.blit(pygame.transform.flip(self.thorn_image, thorn2.flip, False),(thorn2.rect.x,340))
변수 정하기 및 초기화 선언

배경음악

->저작자 이름: Makai Symphony
->곡 제목: Dragon Castle
->링크: https://www.chosic.com/download-audio/29545/

효과음

->링크:https://youtubelab.tistory.com/entry/%EC%B4%9D%EC%86%8C%EB%A6%AC%ED%9A%A8%EA%B3%BC%EC%9D%8C-%EC%B4%9D%EC%9E%A5%EC%A0%84-%EC%86%8C%EB%A6%AC-%ED%9A%A8%EA%B3%BC%EC%9D%8C-%EB%AA%A8%EC%9D%8C-%EB%AC%B4%EB%A3%8C-%EB%8B%A4%EC%9A%B4
pygame.init()

pygame.mixer.init()
#배경음악
pygame.mixer.music.load('sound_source/Dragon_Castle.mp3')
# 배경음악 재생 (루프 설정: -1은 무한 반복)
pygame.mixer.music.play(-1)

sound1 = pygame.mixer.Sound('sound_source/shot.wav')
sound2 = pygame.mixer.Sound('sound_source/reload.wav')

Screen = pygame.display.set_mode((960,720)) # 화면 창 크기
pygame.display.set_caption("SHOOTING GAME") # 게임 이름

ScreenWidth = Screen.get_size()[0] # 화면 가로 길이
ScreenHeight = Screen.get_size()[1] # 화면 세로 길이

start_button = pygame.image.load('image_source/start1.png')
exit_button = pygame.image.load('image_source/exit1.png')
exit_button_image = pygame.transform.scale(exit_button, (60, 60))
img_main_character = pygame.image.load('image_source/soldier_idle.png') # 메인 캐릭터 이미지
background = pygame.image.load('image_source/BG.png') # 배경 이미지
hp_bar = pygame.image.load('image_source/hp_bar.png')
boss_hp_bar = pygame.image.load('image_source/blood_red_bar.png')
boss_hp_bar_image = pygame.transform.scale(boss_hp_bar, (int(boss_hp_bar.get_width() * 2), int(boss_hp_bar.get_height())))

fps = pygame.time.Clock() # 초당 프레임

#글씨 크기 생성
font1 = pygame.font.SysFont('Impact', 23) 
font2 = pygame.font.SysFont('Comic Sans MS', 60)
font3 = pygame.font.SysFont('Impact', 150)
font4 = pygame.font.SysFont('Comic Sans MS', 20)
font5 = pygame.font.SysFont('Impact', 100) 

point = 0 #점수

boss_damage = 0 #보스 데미지

#좌우 움직임 여부
moving_left = False
moving_right = False

current_screen = MenuScreen(Screen) # 현재 스크린(게임화면 or 메뉴화면)

game_start_clock = False # 게임 시작 시각

bullet_start_clock = False # 총탄 발사 시각

bullet_reload_clock = False # 재장전 시각

game_end_clock = False # 게임 종료 시각

moving_clock = False # 움직임 애니메이션 시각

boss_idle_clock = False # 보스 패턴 시간1

boss_idle_clock2 = False # 보스 패턴 시간2

boss_idle_clock3 = False # 보스 패턴 시간3

moving = False # 움직임 여부

running = None # 뛰기 여부

jumping = False # 점프 여부

shooting = False # 발사 여부

reloading = False # 재장전 여부

game_player = Shooter(1) # 플레이어
boss = Boss(120, 180) # 적
bullet = Bullet(0.06, 8) # 총탄
ground = Ground(int(ScreenWidth),int(ScreenHeight * 1/6)) #땅
thorn1 = Thorn(1)
thorn2 = Thorn(2)

boss.rect.x, boss.rect.y = 840, 0
thorn2.rect.x, thorn2.rect.y = -int(thorn2.thorn.get_width() * 1/2 + 700), -200
boss_sprites = pygame.sprite.Group()
boss_sprites.add(boss, thorn2)

bullet_sprites = pygame.sprite.Group()
bullet_sprites.add(bullet)

키 입력 받기

play = True # 게임 창 OPEN/CLOSE

while play:
    deltaTime = fps.tick(60) # 초당 60 프레임
    # 글씨
    text1 = font4.render('SCORE:' + str(round(point, 1)), True, (0,0,0)) # 점수
    text2 = font2.render('START', True, (50,155,200)) # 시작
    text3 = font2.render('EXIT', True, (50,155,200)) # 나가기
    text4 = font1.render('AMMO :', True, (192,192,192)) # 남은 총알 수 
    text5 = font5.render('SHOOTING GAME', True, (255,215,0)) # 제목
    text6 = font3.render('GAME OVER', True, (255,255,255)) # 게임오버
    text7 = font3.render('GAME Clear', True, (255,255,255)) # 게임클리어
    # 방향키,마우스 클릭 여부에 따라 이벤트 발생
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            play = False
        if event.type == pygame.MOUSEBUTTONUP:
            x, y = event.pos
            next_screen = current_screen.handle_event(event)
            if pygame.Rect(int(ScreenWidth - exit_button_image.get_width()), int(ScreenHeight - exit_button_image.get_height()), exit_button_image.get_width(), exit_button_image.get_height()).collidepoint(x, y):
                play = False
            if next_screen == "game":
                game_start_clock = pygame.time.get_ticks()
                boss_idle_clock = pygame.time.get_ticks()
                boss_idle_clock2 = pygame.time.get_ticks()
                boss_idle_clock3 = pygame.time.get_ticks()
                moving = True
                current_screen = GameScreen(Screen)
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_a or event.key == pygame.K_LEFT:
                moving_left = True
                moving_clock = pygame.time.get_ticks()
            if event.key == pygame.K_d or event.key == pygame.K_RIGHT:
                moving_right = True
                moving_clock = pygame.time.get_ticks()
            if event.key == pygame.K_RSHIFT or event.key == pygame.K_LSHIFT:
                running = True
                      if event.key == pygame.K_w or event.key == pygame.K_UP or event.key == pygame.K_SPACE:
                jumping = True
            if event.key == pygame.K_k:
                if bullet.count > 0:
                    shooting = True
                    bullet.count += -1
                    bullet_start_clock = pygame.time.get_ticks()
                    point += 1
                    sound1.play()
            if event.key == pygame.K_r:
                if not bullet_reload_clock:
                    reloading = True
                    bullet_reload_clock = pygame.time.get_ticks()
                    sound2.play()
        if event.type == pygame.KEYUP:
            if event.key == pygame.K_a or event.key == pygame.K_LEFT:
                moving_left = False
            if event.key == pygame.K_d or event.key == pygame.K_RIGHT:
                moving_right = False
            if event.key == pygame.K_RSHIFT or event.key == pygame.K_LSHIFT:
                running = False
실행하기

 #게임 화면과 메뉴화면을 바꾸는 코드
    current_screen.draw()
    # 움직임 관련 그리기
    if moving:
        boss.boss_animation(boss_idle_clock)
        boss.boss_draw()
        thorn2.thorn_draw(boss_idle_clock3)
        if pygame.time.get_ticks() - boss_idle_clock >= 600:
            boss_idle_clock = pygame.time.get_ticks()
        if pygame.time.get_ticks() - boss_idle_clock2 >= 6000:
            boss_idle_clock2 = pygame.time.get_ticks()
        if pygame.time.get_ticks() - boss_idle_clock3 >= 18000:
            boss_idle_clock3 = pygame.time.get_ticks()
        if pygame.sprite.spritecollide(game_player, boss_sprites, False):
            game_player.player_hp -= 3
            if point > 0.01:
                point += -0.2
        game_player.player_move(1, moving_left, moving_right, moving_clock)
        bullet.bullet_move_direction(moving_left, moving_right)
        game_player.player_draw()
        if shooting:
            bullet.bullet_draw()
            if pygame.sprite.spritecollide(bullet, boss_sprites, False):
                boss_damage -= 1
            if pygame.time.get_ticks() - bullet_start_clock >= 1000:
                shooting = False
        if reloading:
            bullet.bullet_reload()
            if pygame.time.get_ticks() - bullet_reload_clock >= 2000:
                reloading = False
                bullet_reload_clock = False
        if pygame.time.get_ticks() - moving_clock >= 500:
            moving_clock = pygame.time.get_ticks()
    # 점프 관련 그리기
    if jumping:
        game_player.player_jump(-12)
        if game_player.rect.y + Screen.get_size()[1] * 5/6 - game_player.image.get_height() >= Screen.get_size()[1] * 5/6 - game_player.image.get_height():
            jumping = False
    
    # 게임 오버시 창 끄기
    if game_player.player_hp <= 0:
        if not game_end_clock:
            game_end_clock = pygame.time.get_ticks()
        if pygame.time.get_ticks() - game_end_clock >= 2000:
            play = False
    if boss_damage <= -570:
        if not game_end_clock:
            game_end_clock = pygame.time.get_ticks()
        if pygame.time.get_ticks() - game_end_clock >= 2000:
            play = False

    pygame.display.update()

pygame.quit()
