import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *

def draw_square(size, color):
    glColor3fv(color)
    glBegin(GL_QUADS)
    glVertex2f(-size, -size)
    glVertex2f(size, -size)
    glVertex2f(size, size)
    glVertex2f(-size, size)
    glEnd()

def draw_object():
    # Outermost - darkest green
    draw_square(2.0, (0.0, 0.3, 0.0))
    # Next layer - medium dark green
    draw_square(1.5, (0.0, 0.5, 0.0))
    # Next layer - lighter green
    draw_square(1.0, (0.0, 0.7, 0.0))
    # Innermost - lightest green (midpoint)
    draw_square(0.5, (0.0, 0.9, 0.0))

def main():
    pygame.init()
    display = (800, 600)
    pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
    pygame.display.set_caption("05 Lab 1 - 2D Gradient Square")

    glMatrixMode(GL_PROJECTION)
    glLoadIdentity()
    gluOrtho2D(-3, 3, -3, 3)  # 2D orthographic view

    glMatrixMode(GL_MODELVIEW)
    glLoadIdentity()

    clock = pygame.time.Clock()
    angle = 0
    running = True

    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False

        glClear(GL_COLOR_BUFFER_BIT)

        glLoadIdentity()
        glRotatef(angle, 0, 0, 1)  # rotate in 2D plane
        angle += 0.5  # slow rotation

        draw_object()

        pygame.display.flip()
        clock.tick(60)

    pygame.quit()

if __name__ == "__main__":
    main()
