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

# Function to draw a cube with vertex gradients
def draw_gradient_cube(base_brightness):
    glBegin(GL_TRIANGLES)
    for tri in triangles:
        for vertex in tri:
            # Assign gradient colors based on vertex position
            x, y, z = vertices[vertex]
            brightness = base_brightness * (0.7 + (x + y + z) / 6.0)
            glColor3f(0.0, max(0.0, min(1.0, brightness)), 0.0)
            glVertex3fv(vertices[vertex])
    glEnd()

def draw_object():
    # Outermost cube (darkest)
    glPushMatrix()
    glScalef(2.0, 2.0, 2.0)
    glTranslatef(0, 0, -0.5)
    draw_gradient_cube(0.3)
    glPopMatrix()

    # 2nd cube
    glPushMatrix()
    glScalef(1.6, 1.6, 1.6)
    glTranslatef(0, 0, -0.4)
    draw_gradient_cube(0.45)
    glPopMatrix()

    # 3rd cube
    glPushMatrix()
    glScalef(1.2, 1.2, 1.2)
    glTranslatef(0, 0, -0.3)
    draw_gradient_cube(0.6)
    glPopMatrix()

    # 4th cube
    glPushMatrix()
    glScalef(0.8, 0.8, 0.8)
    glTranslatef(0, 0, -0.2)
    draw_gradient_cube(0.75)
    glPopMatrix()

    # Innermost cube (brightest)
    glPushMatrix()
    glScalef(0.4, 0.4, 0.4)
    glTranslatef(0, 0, 0.0)
    draw_gradient_cube(0.9)
    glPopMatrix()

def main():
    pygame.init()
    display = (800, 600)
    pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
    pygame.display.set_caption("05 Lab 1")

    glEnable(GL_DEPTH_TEST)
    gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
    glTranslatef(0.0, 0.0, -10)

    clock = pygame.time.Clock()
    angle = 0
    running = True

    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)

        glPushMatrix()
        angle += 1
        glRotatef(angle, 1, 1, 0)
        draw_object()
        glPopMatrix()

        pygame.display.flip()
        clock.tick(60)

    pygame.quit()

if __name__ == "__main__":
    main()
