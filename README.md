# Nädal 09–10: Publisher & Subscriber (rclpy) – `py_pubsub`

Selle nädala eesmärk on luua ROS 2 **publisher** ja **subscriber**
Pythoni abil ning panna nad omavahel suhtlema topicu kaudu.

## Õpiväljundid
- lood ROS 2 Python paketi (`ament_python`)
- lisad vajalikud sõltuvused `package.xml` faili
- seadistad `entry_points` nii, et `ros2 run` leiab käivitatavad node'id
- käivitad pub/sub süsteemi kahes terminalis

## Eeldused
- ROS 2 Humble keskkond töötab (vt nädal 01–02).

---

## Ülesanne A: loo pakett (kohustuslik)

```bash
cd /workspace/ros2_ws/src
ros2 pkg create --build-type ament_python --license Apache-2.0 py_pubsub
```

---

## Ülesanne B: lisa publisher node (kohustuslik)

1) Mine Pythoni paketi kausta:
```bash
cd /workspace/ros2_ws/src/py_pubsub/py_pubsub
```

2) Laadi alla ROS 2 näite "talker" kood:
```bash
wget https://raw.githubusercontent.com/ros2/examples/humble/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py
```

Fail peab nüüd eksisteerima: `publisher_member_function.py`

---

## Ülesanne C: lisa subscriber node (kohustuslik)

Samast kaustast laadi alla ROS 2 näite "listener" kood:
```bash
wget https://raw.githubusercontent.com/ros2/examples/humble/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py
```

Fail peab nüüd eksisteerima: `subscriber_member_function.py`

---

## Ülesanne D: uuenda `package.xml` sõltuvused ja metaandmed (kohustuslik)

Mine paketi juurkausta:
```bash
cd /workspace/ros2_ws/src/py_pubsub
```

Ava `package.xml` ja täida:
- `<description>` (ei tohi olla TODO)
- `<maintainer>` + email (sinu nimi ja e-post)
- `<license>` (Apache License 2.0 või Apache-2.0)

Lisa ka sõltuvused (vastavalt import'idele):
```xml
<exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```

---

## Ülesanne E: lisa `entry_points` `setup.py` faili (kohustuslik)

Ava `setup.py` ja veendu, et metaandmed klapivad `package.xml`-iga
(`maintainer`, `maintainer_email`, `description`, `license`). Seejärel
lisa `entry_points` alla mõlemad käivitatavad node'id:

```python
entry_points={
    'console_scripts': [
        'talker = py_pubsub.publisher_member_function:main',
        'listener = py_pubsub.subscriber_member_function:main',
    ],
},
```

---

## Ülesanne F: ehita ja käivita (kohustuslik)

1) Mine workspace'i juurkausta:
```bash
cd /workspace/ros2_ws
```

2) Kontrolli sõltuvusi:
```bash
rosdep install -i --from-path src --rosdistro humble -y
```

3) Ehita ainult see pakett:
```bash
colcon build --packages-select py_pubsub
```

4) Source'i setup:
```bash
source install/setup.bash
```

### Käivitamine (2 terminali)

**Terminal 1 (talker):**
```bash
ros2 run py_pubsub talker
```

**Terminal 2** (uus terminal samasse konteinerisse: `docker exec -it <container-name> bash` host-arvutis):
```bash
source /workspace/ros2_ws/install/setup.bash
ros2 run py_pubsub listener
```

Peatamiseks vajuta mõlemas terminalis: **Ctrl + C**

## Kontrollpunktid

- [ ] `py_pubsub/py_pubsub/publisher_member_function.py` sisaldab klassi `MinimalPublisher`.
- [ ] `py_pubsub/py_pubsub/subscriber_member_function.py` sisaldab klassi `MinimalSubscriber`.
- [ ] `package.xml` sisaldab `rclpy` ja `std_msgs` sõltuvusi ja täidetud metaandmeid.
- [ ] `colcon build --packages-select py_pubsub` läbib veata.
- [ ] `talker`/`listener` käivitamisel näed vastavalt "Publishing:" ja "I heard:" ridu.

## Esitamine

Paki `ros2_ws/src/py_pubsub` kaust ZIP-failiks ja lae üles Moodle'isse
selle nädala ülesande juurde.
