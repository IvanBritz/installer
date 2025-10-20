import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *

# Cube vertices
vertices = [
    [1, 1, -1],
    [1, -1, -1],
    [-1, -1, -1],
    [-1, 1, -1],
    [1, 1, 1],
    [1, -1, 1],
    [-1, -1, 1],
    [-1, 1, 1]
]

# Cube edges (optional)
edges = (
    (0,1), (1,2), (2,3), (3,0),
    (4,5), (5,6), (6,7), (7,4),
    (0,4), (1,5), (2,6), (3,7)
)

# Each cube face has 4 vertices
faces = (
    (0,1,2,3),  # Back
    (4,5,6,7),  # Front
    (0,1,5,4),  # Right
    (2,3,7,6),  # Left
    (1,2,6,5),  # Bottom
    (0,3,7,4)   # Top
)

# 4 primary colors (repeated across faces)
colors = [
    (1, 0, 0),  # Red
    (0, 1, 0),  # Green
    (0, 0, 1),  # Blue
    (1, 1, 0)   # Yellow
]

def draw_colored_cube():
    glBegin(GL_QUADS)
    for i, face in enumerate(faces):
        glColor3fv(colors[i % 4])
        for vertex in face:
            glVertex3fv(vertices[vertex])
    glEnd()

def draw_object():
    # Single 3D cube
    draw_colored_cube()

def main():
    pygame.init()
    display = (800, 600)
    pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
    pygame.display.set_caption("05 Lab 1 - 3D Movement & 4 Colors")

    glEnable(GL_DEPTH_TEST)
    gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
    glTranslatef(0.0, 0.0, -7)

    clock = pygame.time.Clock()
    running = True

    # Movement, rotation, scale
    x_move, y_move, z_move = 0, 0, 0
    x_rot, y_rot = 0, 0
    scale = 1.0

    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False

        keys = pygame.key.get_pressed()

        # Translation (move)
        if keys[K_a]: x_move -= 0.1  # Left
        if keys[K_d]: x_move += 0.1  # Right
        if keys[K_w]: y_move += 0.1  # Up
        if keys[K_s]: y_move -= 0.1  # Down
        if keys[K_z]: z_move += 0.1  # Forward (closer)
        if keys[K_x]: z_move -= 0.1  # Backward (away)

        # Rotation
        if keys[K_q]: y_rot -= 2     # Rotate left
        if keys[K_e]: y_rot += 2     # Rotate right
        if keys[K_r]: x_rot -= 2     # Rotate up
        if keys[K_f]: x_rot += 2     # Rotate down

        # Scale
        if keys[K_UP]: scale *= 1.01
        if keys[K_DOWN]: scale *= 0.99

        # Reset
        if keys[K_SPACE]:
            x_move = y_move = z_move = 0
            x_rot = y_rot = 0
            scale = 1.0

        # Render
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        glPushMatrix()

        # Apply all transformations
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
