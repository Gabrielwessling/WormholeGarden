Make sure all singleton managers persist and don’t get duplicated or lost when scenes reload.
- Use DontDestroyOnLoad(gameObject) in Awake() to keep the object alive.
- In Awake(), check if an instance already exists—if so, destroy the duplicate.
- For extra safety, add a [RuntimeInitializeOnLoadMethod] that checks if the singleton exists after a scene/domain reload, and creates it if missing. This helps especially in the Unity Editor or with domain reloads.
---