import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *

# Cube vertices
vertices = [
    [1, 1, 1], [1, 1, -1],
    [1, -1, -1], [1, -1, 1],
    [-1, 1, 1], [-1, -1, -1],
    [-1, -1, 1], [-1, 1, -1]
]

# Triangles (12 total)
triangles = [
    [0, 1, 2], [0, 2, 3],
    [4, 6, 5], [4, 5, 7],
    [0, 3, 6], [0, 6, 4],
    [1, 7, 5], [1, 5, 2],
    [0, 4, 7], [0, 7, 1],
    [3, 2, 5], [3, 5, 6]
]

def draw_cube():
    glBegin(GL_TRIANGLES)
    for tri in triangles:
        for vertex in tri:
            glVertex3fv(vertices[vertex])
    glEnd()

def draw_object():
    # Cube 1 - Center (Red)
    glPushMatrix()
    glColor3f(1, 0, 0)
    glScalef(1.2, 1.2, 1.2)
    draw_cube()
    glPopMatrix()

    # Cube 2 - Top (Green)
    glPushMatrix()
    glColor3f(0, 1, 0)
    glTranslatef(0, 3, 0)
    glScalef(0.6, 0.6, 0.6)
    draw_cube()
    glPopMatrix()

    # Cube 3 - Left (Blue)
    glPushMatrix()
    glColor3f(0, 0, 1)
    glTranslatef(-3, -2, 0)
    glScalef(0.8, 0.8, 0.8)
    draw_cube()
    glPopMatrix()

    # Cube 4 - Right (Yellow)
    glPushMatrix()
    glColor3f(1, 1, 0)
    glTranslatef(3, -2, 0)
    glScalef(0.8, 0.8, 0.8)
    draw_cube()
    glPopMatrix()

def main():
    pygame.init()
    display = (800, 600)
    pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
    pygame.display.set_caption("05 Lab 1 - Movement Enabled")

    glEnable(GL_DEPTH_TEST)
    gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
    glTranslatef(0.0, 0.0, -15)

    clock = pygame.time.Clock()
    running = True

    # Movement and rotation variables
    x_move = 0
    y_move = 0
    z_move = 0
    x_rot = 0
    y_rot = 0
    scale = 1.0

    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False

        keys = pygame.key.get_pressed()

        # Movement controls
        if keys[K_a]:
            x_move -= 0.05
        if keys[K_d]:
            x_move += 0.05
        if keys[K_w]:
            y_move += 0.05
        if keys[K_s]:
            y_move -= 0.05

        # Rotation controls
        if keys[K_q]:
            y_rot += 2
        if keys[K_e]:
            y_rot -= 2
        if keys[K_r]:
            x_rot += 2
        if keys[K_f]:
            x_rot -= 2

        # Scaling
        if keys[K_z]:
            scale *= 0.99
        if keys[K_x]:
            scale *= 1.01

        # Reset
        if keys[K_SPACE]:
            x_move = y_move = z_move = 0
            x_rot = y_rot = 0
            scale = 1.0

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)

        glPushMatrix()

        # Apply transformations
        glTranslatef(x_move, y_move, z_move)
        glScalef(scale, scale, scale)
        glRotatef(x_rot, 1, 0, 0)
        glRotatef(y_rot, 0, 1, 0)

        draw_object()

        glPopMatrix()

        pygame.display.flip()
        clock.tick(60)

    pygame.quit()

if __name__ == "__main__":
    main()
