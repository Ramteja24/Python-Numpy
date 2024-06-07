import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
from ipywidgets import interact, FloatSlider

# Define mathematical functions to visualize
def sphere(x, y, z):
    return x**2 + y**2 + z**2

def saddle(x, y, z):
    return x**2 - y**2 - z**2

# Generate data points
def generate_data(func, x_range=(-5, 5), y_range=(-5, 5), z_range=(-5, 5), num_points=100):
    x = np.linspace(*x_range, num_points)
    y = np.linspace(*y_range, num_points)
    z = np.linspace(*z_range, num_points)
    X, Y, Z = np.meshgrid(x, y, z, indexing='ij')
    data = func(X, Y, Z)
    return X, Y, Z, data

# Plot 3D visualization
def plot_3d_surface(X, Y, Z, data, title):
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')
    surf = ax.plot_surface(X, Y, Z, cmap='viridis', edgecolor='none', alpha=0.7)
    fig.colorbar(surf, ax=ax, shrink=0.5, aspect=5)
    ax.set_title(title)
    ax.set_xlabel('X')
    ax.set_ylabel('Y')
    ax.set_zlabel('Z')
    plt.show()

# Define interactive visualization function
def interactive_visualization(func):
    @interact(x_range=FloatSlider(min=-10, max=10, step=0.1, value=(-5, 5), description='X Range'),
              y_range=FloatSlider(min=-10, max=10, step=0.1, value=(-5, 5), description='Y Range'),
              z_range=FloatSlider(min=-10, max=10, step=0.1, value=(-5, 5), description='Z Range'))
    def update(x_range, y_range, z_range):
        X, Y, Z, data = generate_data(func, x_range, y_range, z_range)
        plot_3d_surface(X, Y, Z, data, title=func.__name__)

# Mathematical functions to visualize
functions = {
    'Sphere': sphere,
    'Saddle': saddle
}

# Generate and plot surfaces for each function
for name, func in functions.items():
    print(f'Generating surface for {name}...')
    interactive_visualization(func)
