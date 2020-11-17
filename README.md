# Speedy

import pygame
import random
import time

pygame.init() 

# width and height of the screen 
screen = pygame.display.set_mode((500,450))


running = True

#Title and Icon
pygame.display.set_caption("Wild Driving")

#loading the icon of the bicyle
icon = pygame.image.load('car(2).png')
pygame.display.set_icon(icon)

#clock = pygame.time.clock()

gameImage = pygame.image.load('car(2).png')
backgroundPic = pygame.image.load('road2.png')

grassImg = pygame.image.load('grass2.png')
roadstrips = pygame.image.load('roadstrips.png')
enemyPic = pygame.image.load("pinkcar.png")
gameBackground = pygame.image.load("track.jpg")




def intro_loop():
    intro = True
    while intro:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
                sys.exit()

        screen.blit(gameBackground,(0,0))
        largetext = pygame.font.Font("freesansbold.ttf",50)
        TextSurf,TextRect = textObjects("Dynamic Car", largetext)
        TextRect.center = (200,100)
        screen.blit(TextSurf,TextRect)
        button("START", 100, 380, 80, 30,[25,125,100],[255,255,255],"play")
        button("QUIT", 380, 380, 80, 30, [25,125,100],[255,255,255],"quit")
        button("INSTRUCTION", 200, 380, 150, 30, [25,125,100],[255,255,255],"intro")
        pygame.display.update()
        #clock.tick(50)


def button(msg,x,y,w,h,ic,ac, action = None):
    mouse = pygame.mouse.get_pos()
    click = pygame.mouse.get_pressed()
    if x + w > mouse[0] > x and y + h > mouse[1] > y:
        pygame.draw.rect(screen,ac,(x,y,w,h))
        if click[0] == 1 and action != None:
            if action == "play":
                gameloop()
            elif action == "quit":
                pygame.quit()
                quit()
                sys.exit()
            elif action == "intro":
                introduction()
            elif action == "menu":
                intro_loop()
    else:
        pygame.draw.rect(screen,ic,(x,y,w,h))
    smallText = pygame.font.Font("freesansbold.ttf",20)
    TextSurf,TextRect = textObjects(msg, smallText)
    TextRect.center = ((x+(w/2)),(y+(h/2)))
    screen.blit(TextSurf,TextRect)
def introduction():
    introduction = True
    while introduction:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
                sys.exit()

        screen.blit(gameBackground,(0,0))
        smallText = pygame.font.Font("freesansbold.ttf",20)
        TextSurf,TextRect = textObjects("This is  a typing race game:", smallText)
        TextRect.center = (200,100)
        textsurf,textrect = textObjects("You will need to type as fast as possible to win.",smallText)
        textrect.center = (230,150)
        screen.blit(TextSurf,TextRect)
        screen.blit(textsurf,textrect)
        button("BACK", 300,380, 80,30,[25,125,100],[255,255,255],"menu")
        pygame.display.update()
        #clock.tick(30)
def background():
    screen.blit(backgroundPic,(100,-5))
def movingroad():
    screen.blit(roadstrips,(180,5))
def obstacle(obstacleStartX, obstacleStartY, obs):
    if obs == 0:
        obs_pic = pygame.image.load("pinkcar.png")
    elif obs == 1:
        obs_pic = pygame.image.load("blackcar.png")
    screen.blit(obs_pic,(obstacleStartX,obstacleStartY))
def player(x,y):
    # blit = draw
    screen.blit(gameImage,(x,y))
def enemy(x1,y1):
    screen.blit(enemyPic,(x1,y1))
def losingMessage(text):
    largeText = pygame.font.Font("freesansbold.ttf",40)
    textsurf,textrect = textObjects(text,largeText)
    textrect.center = ((250),(200))
    screen.blit(textsurf,textrect)
    pygame.display.update()
    time.sleep(2)
    gameloop()
def textObjects(text,font):
    textsurface = font.render(text,True,[0,0,0])
    return textsurface,textsurface.get_rect()
def crash():
    losingMessage("YOU CRASHED!!")
def levelMessage(text):
    largeText = pygame.font.Font("freesansbold.ttf",40)
    textsurf,textrect = textObjects(text,largeText)
    textrect.center = ((250),(200))
    screen.blit(textsurf,textrect)
    pygame.display.update()
    time.sleep(2)
    
def scoreSystem(passed, score):
    font = pygame.font.SysFont(None,25)
    text = font.render("Passed: "+ str(passed), True, [255,0,0])
    scoring = font.render("Score: "+ str(score), True, [25,125,100])
    screen.blit(text,(0,30))
    screen.blit(scoring,(0,10))
# Game Loop
def gameloop():
    x = 225
    y = 380
    x_change = 0
    y_change = 0
    playerHeight = 64
    playerWidth = 36
    obstacleSpeed = 2
    obs = 0
    obstacleStartX = random.randrange(140,320)
    obstacleStartY = 0
    obstacleWidth = 36
    obstacleHeight = 56
    passed = 0
    level = 0
    score = 0

    x1 = random.randint(300,600)
    y1 = random.randint(300,600)
    y1_change = -0.3
    
    while running:
        screen.fill((255,255,255))

        
        
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()

            #if keystroke is presses check whether it is right or left
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_change = -2
                if event.key == pygame.K_RIGHT:
                    x_change = 2
                if event.key == pygame.K_UP:
                    obstacleSpeed += 1
                if event.key == pygame.K_DOWN:
                    obstacleSpeed -= 1
            #if key is released
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    x_change = 0
                
        
        x += x_change
        

        background()
        #grass()
        movingroad()
        obstacleStartY -= (obstacleSpeed/3)
        obstacle(obstacleStartX, obstacleStartY, obs)
        obstacleStartY += obstacleSpeed
        player(x,y)
        scoreSystem(passed, score)
        

        
        #checking boundaries of the car
        if x <= 110 or x >= 355:
            crash()
        #updating the scores
        elif obstacleStartY >  450:
            obstacleStartY = 0 - obstacleHeight
            obstacleStartX = random.randrange(140,320)
            obs = random.randrange(0,2)
            passed += 1
            score = passed*10
            if int(passed)%10 == 0:
                level += 1
                obstacleSpeed += 1
                levelMessage("LEVEL " + str(level))
                x += 0.5
        # checking if the car crashes with another car
        elif y < obstacleStartY + obstacleHeight:
            if (x > obstacleStartX) and (x < obstacleStartX + obstacleWidth):
                crash()
            elif (x + playerWidth > obstacleStartX) and (x + playerWidth < obstacleStartX + obstacleWidth):
                crash()

            
       
        
        pygame.display.update()##
        #clock.tick(60)
intro_loop()
gameloop()
pygame.quit()
quit()
