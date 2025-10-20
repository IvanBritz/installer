import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *

# Cube vertices
vertices = [
    [1, 1, 1], [1, 1, -1], [1, -1, -1], [1, -1, 1],
    [-1, 1, 1], [-1, -1, -1], [-1, -1, 1], [-1, 1, -1]
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
    # Cube 1
    glPushMatrix()
    glColor3f(1, 0, 0)  # Red
    glTranslatef(-3, 0, 0)
    glScalef(0.5, 0.5, 0.5)
    draw_cube()
    glPopMatrix()

    # Cube 2
    glPushMatrix()
    glColor3f(0, 1, 0)  # Green
    glTranslatef(3, 0, 0)
    glScalef(0.75, 0.75, 0.75)
    draw_cube()
    glPopMatrix()

    # Cube 3
    glPushMatrix()
    glColor3f(0, 0, 1)  # Blue
    glTranslatef(0, 3, 0)
    glScalef(0.6, 0.6, 0.6)
    draw_cube()
    glPopMatrix()

    # Cube 4
    glPushMatrix()
    glColor3f(1, 1, 0)  # Yellow
    glTranslatef(0, -3, 0)
    glScalef(1.2, 1.2, 1.2)
    draw_cube()
    glPopMatrix()

def main():
    pygame.init()
    display = (800, 600)
    pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
    pygame.display.set_caption("05 Lab 1")

    glEnable(GL_DEPTH_TEST)
    gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
    glTranslatef(0.0, 0.0, -15)

    clock = pygame.time.Clock()
    running = True

    # Keyboard controls
    keys = {
        "left": False, "right": False,
        "rot_up": False, "rot_down": False,
        "rot_left": False, "rot_right": False,
        "scale_in": False, "scale_out": False
    }

    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False
            if event.type == KEYDOWN:
                if event.key == K_a: keys["left"] = True
                if event.key == K_d: keys["right"] = True
                if event.key == K_w: keys["rot_up"] = True
                if event.key == K_s: keys["rot_down"] = True
                if event.key == K_q: keys["rot_left"] = True
                if event.key == K_e: keys["rot_right"] = True
                if event.key == K_z: keys["scale_in"] = True
                if event.key == K_x: keys["scale_out"] = True
            if event.type == KEYUP:
                if event.key == K_a: keys["left"] = False
                if event.key == K_d: keys["right"] = False
                if event.key == K_w: keys["rot_up"] = False
                if event.key == K_s: keys["rot_down"] = False
                if event.key == K_q: keys["rot_left"] = False
                if event.key == K_e: keys["rot_right"] = False
                if event.key == K_z: keys["scale_in"] = False
                if event.key == K_x: keys["scale_out"] = False

        # Transformations
        if keys["left"]: glTranslatef(-0.05, 0, 0)
        if keys["right"]: glTranslatef(0.05, 0, 0)
        if keys["rot_up"]: glRotatef(2, 1, 0, 0)
        if keys["rot_down"]: glRotatef(-2, 1, 0, 0)
        if keys["rot_left"]: glRotatef(2, 0, 1, 0)
        if keys["rot_right"]: glRotatef(-2, 0, 1, 0)
        if keys["scale_in"]: glScalef(0.99, 0.99, 0.99)
        if keys["scale_out"]: glScalef(1.01, 1.01, 1.01)

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        draw_object()  # ✅ Replaces draw_cube()
        pygame.display.flip()
        clock.tick(60)

    pygame.quit()

if __name__ == "__main__":
    main()
