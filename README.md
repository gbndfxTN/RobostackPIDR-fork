# robostack

## Présentation

Ce dépôt contient un workspace ROS 2 organisé pour le projet Robostack (packages dans `src/`). Le repo inclut des fichiers de build et d'installation générés (`build/`, `install/`) et une configuration `pixi.toml` utilisée localement pour lancer des profils.

Voir : [pixi.toml](pixi.toml) et le dossier [src](src/).

---

## Prérequis

- Système : macOS (instructions ci-dessous pour macOS). Adaptation possible pour Linux.
- ROS 2 : installez la distribution utilisée par votre projet (ex : humble, galactic, foxy). Remplacez `<ros2_distro>` par votre distribution.
- Outils : `colcon` (pour builder), `rosdep`, `git`, `python3`.
- (Optionnel) `pixi` : si le projet utilise des profils `pixi` (présence de `pixi.toml`), installez l'outil `pixi` si nécessaire.

Exemples d'installation (macOS / Homebrew) :

```bash
# Exemple : installer ros2 (adapter la commande à la doc officielle pour macOS et votre distro)
# Installer colcon
python3 -m pip install -U colcon-common-extensions
# Installer rosdep
sudo pip3 install -U rosdep
sudo rosdep init || true
rosdep update
# Installer pixi si vous l'utilisez (si disponible via pip)
python3 -m pip install pixi || true
```

Note : suivez la documentation ROS 2 officielle pour l'installation correcte sur macOS et la distribution choisie.

---

## Installation des dépendances du workspace

1. Ouvrez un terminal et positionnez-vous à la racine du dépôt :

```bash
cd /path/to/robostack
```

2. Initialisez `rosdep` (si ce n'est pas déjà fait) :

```bash
sudo rosdep init || true
rosdep update
```

3. Installez les dépendances des packages du workspace :

```bash
rosdep install --from-paths src --ignore-src -r -y
```

Remarque : si certaines dépendances sont spécifiques à macOS ou nécessitent des installateurs système (brew, apt), installez-les selon les messages d'erreur.

---

## Compilation (build)

1. Nettoyage optionnel (si vous souhaitez forcer un build propre) :

```bash
rm -rf build/ install/ log/
```

2. Construire avec `colcon` :

```bash
colcon build --symlink-install
```

3. Après un build réussi, sourcez l'environnement :

```bash
source install/setup.bash
# ou, sur macOS zsh
source install/setup.zsh
# pour réutiliser dans la session courante
```

Note : si vous utilisez l'environnement local généré (`local_setup.*`), vous pouvez sourcer celui-là.

---

## Lancer le projet

Plusieurs méthodes possibles selon les outils/support du projet :

- Avec `pixi` (si disponible) — exemple vu dans l'historique :

```bash
pixi run path-markers
```

`pixi run <profil>` lance le profil décrit dans `pixi.toml`.

- Avec ROS 2 directement :

Si le package fournit un fichier `launch`, utilisez `ros2 launch` :

```bash
# Exemple générique
ros2 launch <package> <launch_file.launch.py>
```

ou pour exécuter un noeud précis :

```bash
ros2 run <package> <node_executable>
```

Remplacez `<package>` et `<node_executable>` par le nom du package/noeud présents dans `src/`.

---

## Commandes utiles

- Rebuild rapide :

```bash
colcon build --symlink-install --packages-select <package_name>
source install/setup.bash
```

- Lister les packages ROS 2 :

```bash
colcon list
```

- Voir les logs de build :

```bash
less log/latest_build/<timestamp>/log/latest_build.log
```

- Exécuter des tests (si présents) :

```bash
colcon test
colcon test-result --verbose
```

---

## Dépannage

- Erreur de dépendances lors de `rosdep install` : installez manuellement les paquets système indiqués (Homebrew sur macOS) puis relancez `rosdep`.
- Erreur lors du sourcing `install/setup.bash` : assurez-vous que la compilation `colcon build` s'est terminée avec succès et que le répertoire `install/` existe.
- Si `pixi` n'est pas trouvé : installez-le via pip (`python3 -m pip install pixi`) ou utilisez les commandes ROS 2 classiques ci-dessus.

---

## Structure du dépôt

- `pixi.toml` : configuration de profils/pipelines si vous utilisez `pixi`.
- `src/` : code source des packages ROS 2.
- `build/`, `install/`, `log/` : dossiers générés par `colcon`.

---

## Contribuer

- Créez une branche feature :

```bash
git checkout -b feat/ma-modif
```

- Soumettez des PRs avec description claire des changements et instructions de test.

---

## Licence

Spécifiez ici la licence du projet (ex : MIT, Apache-2.0). Si elle n'existe pas, ajoutez un fichier `LICENSE` à la racine.

---

Si vous voulez, je peux :
- adapter ce README à une distribution ROS 2 précise (ex : `humble`) ;
- ajouter des exemples de lancement précis en listant les fichiers `launch` dans `src/` ;
- ajouter un script `make run` ou un profil `pixi` prêt à l'emploi.

