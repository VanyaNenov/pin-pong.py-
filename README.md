import pygame as pg
pg.init()


class GameSprite(pg.sprite.Sprite):
    def __init__(self, sprite_image, sprite_x, sprite_y, size_x, size_y, speed):
        super().__init__()
        self.img = pg.transform.scale(pg.image.load(sprite_image), (size_x, size_y))
        self.rect = self.img.get_rect()
        self.rect.x = sprite_x
        self.rect.y = sprite_y
        self.speed = speed

    def draw(self):
        window.blit(self.img, (self.rect.x, self.rect.y))

class Rocket(GameSprite):
    def update1(self):
        keys = pg.key.get_pressed()
        if keys[pg.K_w] and self.rect.y > 0:
            self.rect.y -= self.speed
        elif keys[pg.K_s] and self.rect.y < 300:
            self.rect.y += self.speed

    def update2(self):
        keys = pg.key.get_pressed()
        if keys[pg.K_UP] and self.rect.y > 0:
            self.rect.y -= self.speed
        elif keys[pg.K_DOWN] and self.rect.y < 300:
            self.rect.y += self.speed

class Ball(GameSprite):
    def update(self):
        self.rect.x += speed_x
        self.rect.y += speed_y



window = pg.display.set_mode((700, 500))
clock = pg.time.Clock()

speed_x = 5
speed_y = 5

ball = Ball("ball.png", 300, 200, 70, 70, 10)
player1 = Rocket("rocket.png", 0, 20, 150, 200, 10)
player2 = Rocket("rocket.png", 570, 20, 150, 200, 10)

pg.font.init()
font = pg.font.SysFont("arial", 35)
team1 = font.render('Left Team Lose!', True, (253, 0, 0))
team2 = font.render('Right Team Lose!', True, (253, 0, 0))



game = True
finish = False
while game:
    for e in pg.event.get():
        if e.type == pg.QUIT:
            game = False
    if finish != True:
        window.fill((140, 191, 113))
        player1.update1()
        player2.update2()
        ball.update()
        if ball.rect.y >= 500 - 70:
            speed_y *= -1
        elif ball.rect.y <= 0:
            speed_y *= -1
        if ball.rect.colliderect(player2.rect) or ball.rect.colliderect(player1.rect):
            speed_x *= -1
            speed_y *= 1
        if ball.rect.x < 0:
            finish = True
            window.blit(team1, (250, 250))
            game_over = True
        if ball.rect.x > 700:
            finish = True
            window.blit(team2, (250, 250))
            game_over = True
        player1.draw()
        player2.draw()
        ball.draw()
    pg.display.update()
    clock.tick(40)
