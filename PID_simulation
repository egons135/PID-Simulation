'''
Simple simulation of a helicopter/drone
made to explore each function of a PID controller
Author - egons;
'''
import pygame
import pygame_widgets
from pygame_widgets.slider import Slider
from pygame_widgets.textbox import TextBox
#setup global variables
gravity = 9.8
velocity_decay = 0.3
velocity = 0
velocitx = 0
Height_Set = 0
pre_height = 0
Previous_Height=0
prev_err=0
integral=0
deriv=0
proportion=0



def PID(preset, current, Prop_weight, Integ_weight, Deriv_weight):
    velo=0.0
    global prev_err
    global integral
    global deriv
    global proportion
    error = preset - current
    #proportional part
    if use_proportional:
        proportion=error * Prop_weight 
        velo = proportion   
    #derivative part
    if use_derivative:
        deriv=(error-prev_err)/dt
        velo+=deriv*Deriv_weight
    #integral part
    if use_integral:
        integral+=error*dt
        velo+=integral*Integ_weight
    prev_err=error
    return -velo

pygame.init()
screen = pygame.display.set_mode((1280, 720))
clock = pygame.time.Clock()
running = True
pygame.display.set_caption("PID HEIGHT BALL")
dt = 0
font = pygame.font.Font(None, 50)
avatar_size=60
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GRAY = (200, 200, 200)
GREEN = (0, 200, 0)
BROWN=(148,102,56)
BACKGROUNDCOLOR=(150,150,150)
#init sliders
slidersize=20
sliderlenght=250
TextBoxsize=32
startpos=200
step=70
textbox_pos=250

sliderProp = Slider(screen, 50, startpos, sliderlenght, slidersize, min=0.1, max=10, step=0.1,colour=WHITE,initial=1)
outputSliderProp = TextBox(screen, textbox_pos, startpos-slidersize*2, TextBoxsize*2-15, TextBoxsize, fontSize=20,colour=BACKGROUNDCOLOR,textColour=WHITE,borderColour=BACKGROUNDCOLOR)
label_prop=TextBox(screen, 50, startpos-slidersize*2, 200, 32,textColour=WHITE, fontSize=20,colour=BACKGROUNDCOLOR,borderColour=BACKGROUNDCOLOR)
outputSliderProp.disable()
label_prop.disable()
startpos+=step

sliderIntegral = Slider(screen, 50, startpos, sliderlenght, slidersize, min=0.1, max=10, step=0.1,colour=WHITE,initial=1)
outputSliderIntegral = TextBox(screen, textbox_pos, startpos-slidersize*2, TextBoxsize*2-15, TextBoxsize, fontSize=20,colour=BACKGROUNDCOLOR,textColour=WHITE,borderColour=BACKGROUNDCOLOR)
label_integral=TextBox(screen, 50, startpos-slidersize*2, 200, 32,textColour=WHITE, fontSize=20,colour=BACKGROUNDCOLOR,borderColour=BACKGROUNDCOLOR)
outputSliderIntegral.disable()
label_integral.disable()
startpos+=step


sliderDerivative = Slider(screen, 50, startpos, sliderlenght, slidersize, min=0.1, max=3, step=0.1,colour=WHITE,initial=1)
outputSliderDerivative = TextBox(screen, textbox_pos, startpos-slidersize*2, TextBoxsize*2-15, TextBoxsize, fontSize=20,colour=BACKGROUNDCOLOR,textColour=WHITE,borderColour=BACKGROUNDCOLOR)
label_deriv=TextBox(screen, 50, startpos-slidersize*2, 200, 32,textColour=WHITE, fontSize=20,colour=BACKGROUNDCOLOR,borderColour=BACKGROUNDCOLOR)
outputSliderDerivative.disable()
label_deriv.disable()

# Checkbox positions and states
checkbox_prop_rect = pygame.Rect(50, 50, 20, 20)
checkbox_int_rect = pygame.Rect(50, 90, 20, 20)
checkbox_der_rect = pygame.Rect(50, 130, 20, 20)

use_proportional = False
use_integral = False
use_derivative = False

#loading avatar
avatar_image = pygame.image.load('avatar.png')  # Load your image here
avatar_image = pygame.transform.scale(avatar_image, (avatar_size, avatar_size))

player_pos = pygame.Vector2(screen.get_width() / 2, screen.get_height() / 2)

def draw_checkbox(rect, checked, label):
    pygame.draw.rect(screen, WHITE, rect, 2)
    if checked:
        pygame.draw.rect(screen, GREEN, rect.inflate(-4, -4))
    label_surface = font.render(label, True, GREEN)
    screen.blit(label_surface, (rect.right + 10, rect.y - 2))

while running:
    # Update physics
    Previous_Height=player_pos.y-600
    tempvel = PID(pre_height, abs(player_pos.y - 600), sliderProp.getValue(), sliderIntegral.getValue(), sliderDerivative.getValue())

    player_pos.y += velocity * dt / 3
    player_pos.x += velocitx * dt

    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.MOUSEBUTTONDOWN:
            if checkbox_prop_rect.collidepoint(event.pos):
                use_proportional = not use_proportional
            elif checkbox_int_rect.collidepoint(event.pos):
                use_integral = not use_integral
            elif checkbox_der_rect.collidepoint(event.pos):
                use_derivative = not use_derivative

    # Clear screen
    screen.fill(BACKGROUNDCOLOR)

    # Draw ground
    pygame.draw.rect(screen, BROWN, (0, screen.get_height() - 100, screen.get_width(), screen.get_height() / 4))

    # Collision with ground
    if player_pos.y > screen.get_height() - 120:
        player_pos.y = screen.get_height() - 120
        velocity = 0

    # Draw player
    #pygame.draw.circle(screen, "red", player_pos, 20)
    screen.blit(avatar_image, player_pos - pygame.Vector2(avatar_image.get_width() / 2, avatar_image.get_height() / 2))

    # Render text
    x_pos_header = font.render("X position: %i" % player_pos.x, False, WHITE)
    y_pos_header = font.render("Y position: %i" % player_pos.y, False, WHITE)
    Height = font.render("Height: %i" % (abs(player_pos.y - 600)), False, WHITE)
    PHeight = font.render("Preset height: %i" % pre_height, False, WHITE)
    velodisp = font.render("Velocity: %s" % round(velocity,2), False, WHITE)
    tvel = font.render("Proportional: %s" % round(-proportion,2), False, WHITE)
    tvin = font.render("Integral: %s" % round(integral,2), False, WHITE)
    tvdir = font.render("Derivative: %s" % round(deriv,2), False, WHITE)
    
    velocity += tempvel
    
    
    
    #drawing HUD displays
    screendisplayoffset=350
    screen.blit(x_pos_header, (screen.get_width() - screendisplayoffset, 30))
    screen.blit(y_pos_header, (screen.get_width() - screendisplayoffset, 60))
    screen.blit(Height, (screen.get_width() - screendisplayoffset, 90))
    screen.blit(PHeight, (screen.get_width() - screendisplayoffset, 120))
    screen.blit(velodisp, (screen.get_width() - screendisplayoffset, 150))
    screen.blit(tvel, (screen.get_width() - screendisplayoffset, 180))
    screen.blit(tvin, (screen.get_width() - screendisplayoffset, 210))
    screen.blit(tvdir, (screen.get_width() - screendisplayoffset, 240))
    outputSliderProp.setText(str(round(sliderProp.getValue(),1)))
    outputSliderIntegral.setText(str(round(sliderIntegral.getValue(),1)))
    outputSliderDerivative.setText(str(round(sliderDerivative.getValue(),1)))
    label_prop.setText("Proportional Weight")
    label_integral.setText("Integral Weight")
    label_deriv.setText("Derivative Weight")

    # Draw checkboxes
    draw_checkbox(checkbox_prop_rect, use_proportional, "Proportional")
    draw_checkbox(checkbox_int_rect, use_integral, "Integral")
    draw_checkbox(checkbox_der_rect, use_derivative, "Derivative")

    # Movement controls
    keys = pygame.key.get_pressed()
    if keys[pygame.K_w]:
        velocity -= 25
    if keys[pygame.K_s]:
        velocity += 25
    if (player_pos.y < screen.get_height() - avatar_size*2) and (0 < player_pos.x < screen.get_width()):
        if keys[pygame.K_d]:
            velocitx += 25
        if keys[pygame.K_a]:
            velocitx -= 25
    else:
        velocitx = 0

    if keys[pygame.K_1]:
        pre_height += 1
    if keys[pygame.K_2]:
        pre_height -= 1
    if keys[pygame.K_r]:
        player_pos.x = 640
        player_pos.y = 600
        velocity = 0
        velocitx = 0
        deriv=0
        integral=0

    if pre_height < 0:
        pre_height = 0

    # Friction
    if velocitx > 0:
        velocitx -= velocity_decay
    if velocitx < 0:
        velocitx += velocity_decay
    if velocity > 0:
        velocity -= velocity_decay
    if velocity < 0:
        velocity += velocity_decay

    # Screen bounds
    if player_pos.x < 0:
        player_pos.x = 1
    if player_pos.x > screen.get_width():
        player_pos.x = screen.get_width() - 1

    
    velocity += gravity

    # Update display
    pygame_widgets.update(event)  # Update all pygame_widgets
    pygame.display.update()  # Update the display

    pygame.display.flip()
    dt = clock.tick(60) / 1000

pygame.quit()
