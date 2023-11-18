extends KinematicBody

var speed = 5.0
var gravity = -9.8
var velocity = Vector3()

var sensitivity = 0.2 # حساسية الماوس
var camera_rotation = Vector2()

func _process(delta):
    # تحريك اللاعب بناءً على الإدخال
    var move_direction = Vector3()

    if Input.is_action_pressed("ui_up"):
        move_direction.z -= 1
    if Input.is_action_pressed("ui_down"):
        move_direction.z += 1
    if Input.is_action_pressed("ui_left"):
        move_direction.x -= 1
    if Input.is_action_pressed("ui_right"):
        move_direction.x += 1

    # تحديد اتجاه الحركة وتطبيق السرعة
    move_direction = move_direction.normalized()
    velocity = move_direction * speed

    # تحديث حركة اللاعب باستخدام السرعة والتصادمات
    velocity.y += gravity * delta
    velocity = move_and_slide(velocity, Vector3(0, 1, 0))

    # حركة الكاميرا بناءً على حركة الماوس
    var mouse_motion = Input.get_mouse_motion()
    camera_rotation.x -= mouse_motion.y * sensitivity
    camera_rotation.y -= mouse_motion.x * sensitivity

    camera_rotation.x = clamp(camera_rotation.x, -90, 90) # قيد الحركة على محور X

    $Camera.rotation_degrees.x = camera_rotation.x
    $Camera.rotation_degrees.y = camera_rotation.y
