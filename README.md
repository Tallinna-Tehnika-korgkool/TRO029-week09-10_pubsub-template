# Week 09–10: Publisher & Subscriber (rclpy) – `py_pubsub`

Selle nädala eesmärk on luua ROS 2 **publisher** ja **subscriber** Pythoni abil ning panna nad omavahel suhtlema topic’u kaudu.

## Õpiväljundid
- lood ROS 2 Python paketi (`ament_python`)
- lisad vajalikud sõltuvused `package.xml` faili
- seadistad `entry_points` nii, et `ros2 run` leiab käivitatavad node’id
- käivitad pub/sub süsteemi kahes terminalis

## Eeldused
- ROS 2 Humble keskkond töötab (Docker/Devcontainer).
- Uues terminalis on ROS 2 sätitud (nt `source /opt/ros/humble/setup.bash`).

---

## Ülesanne A: loo pakett (kohustuslik)

1) Mine workspace’i ja `src` kausta:
```bash
cd ~/ros2_ws/src
```

2) Loo pakett nimega **py_pubsub**:
```bash
ros2 pkg create --build-type ament_python --license Apache-2.0 py_pubsub
```

---

## Ülesanne B: lisa publisher node (kohustuslik)

1) Mine Pythoni paketi kausta:
```bash
cd ~/ros2_ws/src/py_pubsub/py_pubsub
```

2) Veendu, et saad alla laadida näitefailid (kui `wget` puudub):
```bash
sudo apt-get update
sudo apt-get install -y wget
```

3) Laadi alla ROS 2 näite “talker” kood:
```bash
wget https://raw.githubusercontent.com/ros2/examples/humble/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py
```

Fail peab nüüd eksisteerima:
- `publisher_member_function.py`

---

## Ülesanne C: lisa subscriber node (kohustuslik)

Samast kaustast laadi alla ROS 2 näite “listener” kood:
```bash
wget https://raw.githubusercontent.com/ros2/examples/humble/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py
```

Fail peab nüüd eksisteerima:
- `subscriber_member_function.py`

---

## Ülesanne D: uuenda `package.xml` sõltuvused ja metaandmed (hindeline)

Mine paketi juurkausta:
```bash
cd ~/ros2_ws/src/py_pubsub
```

Ava `package.xml` ja täida:
- `<description>` (ei tohi olla TODO)
- `<maintainer>` + email (sinu nimi ja e-post)
- `<license>` (Apache License 2.0 või Apache-2.0)

Lisa ka sõltuvused (vastavalt import’idele):
```xml
<exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```

Näide (kohanda enda andmetega):
```xml
<description>Näited minimaalsest avaldajast/tellijast (rclpy)</description>
<maintainer email="you@email.com">Sinu Nimi</maintainer>
<license>Apache License 2.0</license>
```

---

## Ülesanne E: lisa `entry_points` `setup.py` faili (hindeline)

Ava `setup.py` ja veendu, et metaandmed klapivad `package.xml`-iga:
- `maintainer`
- `maintainer_email`
- `description`
- `license`

Seejärel lisa `entry_points` alla mõlemad käivitatavad node’id:

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

1) Mine workspace’i juurkausta:
```bash
cd ~/ros2_ws
```

2) Kontrolli sõltuvusi (hea tava):
```bash
rosdep install -i --from-path src --rosdistro humble -y
```

3) Ehita ainult see pakett:
```bash
colcon build --packages-select py_pubsub
```

4) Source’i setup (uues terminalis või samas):
```bash
source install/setup.bash
```

### Käivitamine (2 terminali)
**Terminal 1 (talker):**
```bash
ros2 run py_pubsub talker
```

**Terminal 2 (listener):**
```bash
source ~/ros2_ws/install/setup.bash
ros2 run py_pubsub listener
```

Peatamiseks vajuta mõlemas terminalis: **Ctrl + C**

---

## Esitamise kord

Harjutuse edukaks esitamiseks **pead töötama oma isiklikus GitHub Classroomi repos**, mis on loodud selle nädala jaoks.

1.  **Ava ülesande link Moodle'ist:**
    - Mine kursuse Moodle'i lehele.
    - Leia sealt selle nädala (Week 09-10) ülesande juurest link oma isikliku GitHub Classroomi repositooriumi loomiseks.

2.  **Klooni enda isiklik repo GitHubist**, mille nimi sisaldab sinu GitHubi kasutajanime.
    Näide:
    ```bash
    git clone https://github.com/Tallinna-Tehnika-korgkool/TRO029-week09-10_pubsub-<sinu_kasutajanimi>
    ```

3.  **Tee kõik ülesanded selles kloonitud repos.**

4.  **Lisa ja `commit`'i oma muudatused** (`ros2_ws` kaust koos sinu paketiga).
    ```bash
    git add .
    git commit -m "docs: Complete week 9-10 exercise"
    git push
    ```

5.  Kui `push` on tehtud, käivituvad **automaatsed testid** (GitHub Actions) sinu repo **Actions** vahekaardil.
    - ✅ **Roheline** ✔️ tähendab, et kõik on korras.
    - ❌ **Punane** ✖️ tähendab, et midagi on puudu või valesti – paranda ja tee uus `commit` ja `push`.

> **NB!** Kui töötad väljaspool oma isiklikku Classroom repo't, siis testid ei tööta ja harjutust ei loeta esitatuks.
