java c
CS770/870 Assignment 8 
• Out: Tuesday, November 5th.
•  Due: Thursday, November 14th.
Overview 
In this assignment, you will write a ray tracer that uses only primary rays (no reflected, refracted or shadow rays). You should be able to render spheres, triangles, and cylinders.
For a 10% bonus, implement shadow rays.
The Scene 
Your rendering should be based on a scene, which is described by the following information:
• one or more materials
• a list of shapes
• a list of lights
• a camera
Pseudocode 
You are free to implement the ray tracer any way you see fit, but I suggest you follow this pseudocode:
foreach pixel (image) {
ray = new_ray (pixel, pixel-eye)
pixel's color = ray_color (ray, scene)
}
ray_color (ray, scene) {
hit = first_hit (ray, scene)
bj = hit.shape
return glossy_color (ray, hit, obj, scene)
}
first_hit (ray, scene) {
list = (empty)
foreach shape (scene) {
if (ray intersects shape at location) {
hit = hitpoint(location, N, shape)
list.append(hit)
}
}
return list.getMinimum()
}
glossy_color (ray, hit, obj, scene) {
lights = scene.light_list
color = (0 0 0)
foreach light (lights) {
color += local_illum (ray.dir, hit.normal, light.dir,
obj.material, light.color)
}
color += obj.material.ambient_color * ambient_light
return color
}
local_illum (rayDir, N, L, mat, I) {
V = -rayDir
R = mirror_direction (L, N)
kd = mat.diffuse_color
ks = mat.specular_color
n = mat.glossiness
return I * (kd * N dot L + ks * (R dot V)^n)
}
mirror_direction (L, N) {
return 2(N dot L)*N - L
}
C++ Classes 
This assignment extends the previous one. The new features include:
• Instead of a vec3 color, each Shape now has an associated Material. The Material class (already implemented) has information for calculating a color using the Phong illumination model:
– ambient reflectance
– difuse reflectance
– specular reflectance
– shininess exponent
• The Hit class now has pointer to Material insted of a simple vec3 color.  Its .set () method has changed accordingly (already implemented).
• The Light class describes a point light source, with this information (already implemented):
– position
– color
• The Shape class and its children Triangle, Sphere and Cylinder all have a Material field too.
• The Camera class is new. It has these fields (all public) (already implemented):
– _eye : eye position
– _lookat : where the camera is looking at
– _up : which way is “up” in the world
– _clip_Left, _clip_Right, _clip_Bottom, _clip_Top, and _clip_Near : these position the image rect- angle.
– _ambient_fraction : how much each light contributes to the over-all ambient light.
• The Caster class has several changes:
– It now has a _camera instance variable. This is a Camera object, which you’ll use to re-compute the VCS→ WCS matrix.
– The method that should update the VCS→ WCS matrix is now called camera_did_move ().
– The ray_color () method should implement the pseudocode above.
– The glossy_color () method should compute the color at the given hit point, taking all the light sources into account, and also the ambient light color.
– The local_illumination () method computes the contribution of ONE light to the hit point.
– The mirror_dir代 写CS770/870 Assignment 8C/C++
代做程序编程语言ection () method computes the reflection R vector, based on the vector L towards the light, and the vector N normal to the hit surface.
– The hits_something() method should return true/false if the given ray hits some object. It’s used for shadow rays. You should call it from ray_color () if_shadowing is‘true.
– The read_scene () method replaces the init_scene (). It reads a scene description from a file (already implemented).
• The Tokenize r class will read a file, and report whitespace-separated tokens.  It has these methods (already implemented):
– next_string() : returns the next token.
– next_number () : returns the next token, converted to a ﬂoat (may throw an exception).
– match (s ) : reads the next token, and throws an exception if it doesn’t match s.
– eof () : returns true at end of file.
– ﬁle_position() : returns a string describing the current line number in the file.
• The Scene_Reader class handles reading the scene description. It includes these methods:
– read_sphere() : reads information for a sphere, and returns a pointer to a new Sphere object (already implemented).
– read_triangle() : reads information for a triangle, and returns a pointer to a new Triangle object. NOT COMPLETE.
– read_cylinder() : reads information for a cylinder aligned with they axis, and returns a pointer to a new Cylinder object. NOT COMPLETE.
– read_material () : reads information for a surface material, and returns a Material object. NOT COM- PLETE.
– read_light() : reads information for a point light source, and returns a Light object. NOT COMPLETE.
– read_camera () : reads information for the camera, and returns a Camera object. NOT COMPLETE.
– read_vec3 () reads three floats, and returns a vec3 (already implemented).
– ﬁnd_named_material () : given a vector of Materials, and a name, returns the Material that matches the name (already implemented).
Controls 
The following keyboard controls are implemented in the Caster_Controller class (which you don’t have to change):
•  UP  /  DOWN  arrow: move eye up/down, keeping look-at point fixed
•  LEFT   /   RIGHT  arrow : move eye le什/right, keeping look-at point fixed
• Shift+UP  /  Shift+DOWN  arrow: move eye forward/backward.
• R  /  Shift+R : decrease/increase the resolution
• C : output the current camera position.
• S : toggle the _shadowing flag.
Additional Information 
I’ve prepared additional information files:
• data-format .pdf: syntax specifications for the scene files.
• illumination .pdf : details on the Phong local illumination.
• rt-faq.pdf : other frequently-asked questions.
Expected Output 
The program caster expects a command-line argument, which is a scene file. If you type
caster scene/pyramid.txt 
on the command line, you should see this:

If you type
caster  scene/snowman .txt 
on the command line (and have implemented shadows), you should see this:

If you type
caster scene/snowman-1light.txt 
on the command line (and have implemented shadows), you should see this:

Submission 
When you are done, go to the mycourses, find the course, upload all the files that you changed (especially caster .cpp, triangle.cpp, sphere.cpp, cylinder.cpp, and scene_ reader .cpp),and click “Submit”.







         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
