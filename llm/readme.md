# DeepSeek-R1
## Installation et exécution avec Ollama
1. Installer Ollama : https://ollama.com/
2. Exécuter `ollama run deepseek-r1`
3. Vérifier l'installation `ollama list`
4. Exécuter `ollama run deepseek-r1` pour démarrer le serveur

Ollama roule en service sur le `http://localhost:11434`

## Arrêter le serveur
1. ollama stop deepseek-r1

## Utiliser Open-webui
Pour avoir accès à un GUI avec Ollama, il suffit d'installer Open-webui :

1. Créer un environnement virtuel python : `python -m venv "open-webui"`
2. Activer l'environnement virtuel : `.\open-webui\Scripts\Activate`
3. Installer Open-webui : `pip install open-webui`
4. Exécuter Open-webui : `open-webui serve`