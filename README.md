using UnityEngine;

public class BirdController : MonoBehaviour
{
    public float flapStrength = 10f;
    public float gravity = 0.5f;
    public float glideFactor = 0.1f;
    public float horizontalSpeed = 2f;

    private Rigidbody2D rb;
    private bool isFlapping = false;
    
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        rb.gravityScale = gravity;
    }

    void Update()
    {
        // Handle flapping (tap to flap, hold to glide)
        if (Input.GetMouseButtonDown(0))
        {
            isFlapping = true;
        }
        if (Input.GetMouseButtonUp(0))
        {
            isFlapping = false;
        }

        if (isFlapping)
        {
            Flap();
        }
        else
        {
            Glide();
        }
    }

    void Flap()
    {
        rb.velocity = Vector2.zero;  // Stop current velocity
        rb.AddForce(Vector2.up * flapStrength, ForceMode2D.Impulse);
    }

    void Glide()
    {
        if (rb.velocity.y < 0)
        {
            rb.velocity = new Vector2(horizontalSpeed, rb.velocity.y * glideFactor);
        }
    }
}
using UnityEngine;

public class ObstacleSpawner : MonoBehaviour
{
    public GameObject obstaclePrefab;
    public float spawnRate = 2f;
    public float minHeight = -3f;
    public float maxHeight = 3f;

    private void Start()
    {
        InvokeRepeating("SpawnObstacle", 0f, spawnRate);
    }

    void SpawnObstacle()
    {
        float spawnHeight = Random.Range(minHeight, maxHeight);
        Vector3 spawnPosition = new Vector3(10f, spawnHeight, 0);
        Instantiate(obstaclePrefab, spawnPosition, Quaternion.identity);
    }
}
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameOverManager : MonoBehaviour
{
    public void GameOver()
    {
        // Display Game Over screen (you can add UI later)
        Debug.Log("Game Over!");
        Time.timeScale = 0;  // Stop the game
    }

    public void RestartGame()
    {
        // Restart the game (reload scene)
        Time.timeScale = 1;
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}
using UnityEngine;

public class EndlessLevelManager : MonoBehaviour
{
    public GameObject background;
    public float scrollSpeed = 1f;

    private void Update()
    {
        // Scroll the background continuously
        background.transform.Translate(Vector3.left * scrollSpeed * Time.deltaTime);

        // Recycle background when it moves off-screen
        if (background.transform.position.x < -10f) // Assuming background width is 10 units
        {
            background.transform.position = new Vector3(10f, background.transform.position.y, background.transform.position.z);
        }
    }
}
using UnityEngine;

public class Collectible : MonoBehaviour
{
    public enum CollectibleType { Feather, Shield, SpeedBoost }

    public CollectibleType collectibleType;

    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            // Handle collection (e.g., add points or apply power-up)
            switch (collectibleType)
            {
                case CollectibleType.Feather:
                    // Add feather to the player's inventory
                    break;
                case CollectibleType.Shield:
                    // Activate shield
                    break;
                case CollectibleType.SpeedBoost:
                    // Increase speed temporarily
                    break;
            }

            // Destroy the collectible after it is picked up
            Destroy(gameObject);
        }
    }
}
