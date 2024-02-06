import numpy as np

from OpenGL.GL import *

from OpenGL.GLUT import *

from OpenGL.GLU import *



# Размер окна

width, height = 600, 600



# Параметры материала

material_ambient = [0.5, 0.5, 0.5, 1.0]

material_diffuse = [0.5, 0.5, 0.5, 1.0]

material_specular = [1.0, 1.0, 1.0, 1.0]

material_shininess = 50.0



# Параметры точечного источника освещения

light_ambient = [0.0, 0.0, 0.0, 1.0]

light_diffuse = [1.0, 1.0, 1.0, 1.0]

light_specular = [1.0, 1.0, 1.0, 1.0]

light_position = [0.0, 0.0, 1.0, 1.0]



# Координаты вершин куба

cube_vertices = np.array([[-0.5, -0.5, -0.5],

[0.5, -0.5, -0.5],

[0.5, 0.5, -0.5],

[-0.5, 0.5, -0.5],

[-0.5, -0.5, 0.5],

[0.5, -0.5, 0.5],

[0.5, 0.5, 0.5],

[-0.5, 0.5, 0.5]])



# Нормали вершин куба

cube_normals = np.array([[0.0, 0.0, -1.0],

[0.0, 0.0, -1.0],

[0.0, 0.0, -1.0],

[0.0, 0.0, -1.0],

[0.0, 0.0, 1.0],

[0.0, 0.0, 1.0],

[0.0, 0.0, 1.0],

[0.0, 0.0, 1.0]])



# Индексы вершин граней куба

cube_indices = np.array([[0, 1, 2, 3],

[4, 5, 6, 7],

[1, 5, 6, 2],

[0, 4, 7, 3],

[3, 2, 6, 7],

[0, 1, 5, 4]])



# Вычисление вектора поверхности

def surface_normal(p1, p2, p3):

v1 = p2 - p1

v2 = p3 - p1

return np.cross(v1, v2)



# Настройка OpenGL

def setup():

glClearColor(0.0, 0.0, 0.0, 1.0)

glEnable(GL_DEPTH_TEST)

glShadeModel(GL_FLAT)



# Обработка нажатий клавиш

def key_pressed(key, x, y):

if key == b'\x1b': # Esc

sys.exit(0)



# Рендеринг куба

def draw_cube(position, color):

glPushMatrix()

glTranslatef(*position)

glColor3f(*color)

glEnableClientState(GL_VERTEX_ARRAY)

glEnableClientState(GL_NORMAL_ARRAY)

glVertexPointer(3, GL_FLOAT, 0, cube_vertices)

glNormalPointer(GL_FLOAT, 0, cube_normals)

for indices in cube_indices:

glBegin(GL_QUADS)

glArrayElement(indices[0])

glArrayElement(indices[1])

glArrayElement(indices[2])

glArrayElement(indices[3])

glEnd()

glDisableClientState(GL_VERTEX_ARRAY)

glDisableClientState(GL_NORMAL_ARRAY)

glPopMatrix()



# Рендеринг сцены

def draw_scene():

glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)



glPushMatrix()

glLoadIdentity()



glLightfv(GL_LIGHT0, GL_POSITION, light_position)

glLightfv(GL_LIGHT0, GL_AMBIENT, light_ambient)

glLightfv(GL_LIGHT0, GL_DIFFUSE, light_diffuse)

glLightfv(GL_LIGHT0, GL_SPECULAR, light_specular)

glEnable(GL_LIGHT0)



glMaterialfv(GL_FRONT, GL_AMBIENT, material_ambient)

glMaterialfv(GL_FRONT, GL_DIFFUSE, material_diffuse)

glMaterialfv(GL_FRONT, GL_SPECULAR, material_specular)

glMaterialf(GL_FRONT, GL_SHININESS, material_shininess)



draw_cube([-0.5, 0.0, 0.0], [1.0, 0.0, 0.0]) # Красный куб

draw_cube([0.5, 0.0, 0.0], [0.0, 1.0, 0.0]) # Зеленый куб



glPopMatrix()



glutSwapBuffers()



# Инициализация GLUT

glutInit()

glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH)

glutInitWindowSize(width, height)

glutCreateWindow(b"Phong shading")

glutDisplayFunc(draw_scene)

glutIdleFunc(draw_scene)

glutKeyboardFunc(key_pressed)



setup()



glutMainLoop()
