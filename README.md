### link

https://github.com/secaids/arvr

# arvr
### EXP 01:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class code : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        transform.RotateAround(Vector3.right, Vector3.up, 40 * Time.deltaTime);
    }
}
```
### EXP 02:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
   public float xForce = 5.0f;
   public float zForce = 5.0f;
   public float yForce = 200.0f;
   void Start()
   {

   }

   // Update is called once per frame
   void Update()
   {
       float x = 0.0f;
       if(Input.GetKey(KeyCode.A))
       {
           x = x - xForce;
       }
       if (Input.GetKey(KeyCode.D))
       {
           x = x + xForce;
       }
       float z = 0.0f;
       if (Input.GetKey(KeyCode.S))
       {
           z = z - zForce;
       }
       if (Input.GetKey(KeyCode.W))
       {
           z = z + zForce;
       }
       float y = 0.0f;
       if(Input.GetKeyDown(KeyCode.Space))
       {
           y = yForce;
       }

       GetComponent<Rigidbody>().AddForce(x, y, z);
   }
}

```
### EXP 03:
#### GAME MANAGER:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
  public Ball Ball;
  public Paddle Paddle;
  public static Vector2 bottomLeft;
  public static Vector2 topRight;
  // Start is called before the first frame update
  void Start()
  {
      bottomLeft = Camera.main.ScreenToWorldPoint(new Vector2(0, 0));
      topRight = Camera.main.ScreenToWorldPoint(new Vector2(Screen.width, Screen.height));
      Instantiate(Ball);

      Paddle Paddle1 = Instantiate(Paddle) as Paddle;
      Paddle Paddle2 = Instantiate(Paddle) as Paddle;

      Paddle1.Init(true);
      Paddle2.Init(false);
  }

  // Update is called once per frame
  void Update()
  {
      
  }
}
```
#### PADDLE
```
using System.Collections;
using System.Collections.Generic;
using System.Threading;
using UnityEngine;

public class Paddle : MonoBehaviour
{
  [SerializeField]
  float speed;
  float height;
  string input;
  public bool isRight;
  // Start is called before the first frame update
  void Start()
  {
      height = transform.localScale.y;
      speed = 6f;

  }
  public void Init(bool isRightPaddle)
  {
      isRight = isRightPaddle;
      Vector2 pos = Vector2.zero;
      if (isRightPaddle)
      {
          pos = new Vector2(GameManager.topRight.x, 0);
          pos -= Vector2.right * transform.localScale.x;
          input = "PaddleRight";
      }
      else
      {
          pos = new Vector2(GameManager.bottomLeft.x, 0);
          pos += Vector2.right * transform.localScale.x;
          input = "PaddleLeft";
      }
      transform.position = pos;
      transform.name = input;
  }

  // Update is called once per frame
  void Update()
  {
      float move = Input.GetAxis(input) * Time.deltaTime * speed;
      if (transform.position.y < GameManager.bottomLeft.y + height / 2 && move < 0)
      {
          move = 0;
      }
      if (transform.position.y > GameManager.topRight.y - height / 2 && move > 0)
      {
          move = 0;
      }
      transform.Translate(move * Vector2.up);
  }
}
```
#### BALL
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Ball : MonoBehaviour
{
  [SerializeField]
  float speed;
  float radius;
  Vector2 direction;
  void Start()
  {
      direction = Vector2.one.normalized;
      radius = transform.localScale.x / 2;
  }
  // Update is called once per frame
  void Update()
  {
      transform.Translate(direction * speed * Time.deltaTime);
      if (transform.position.y < GameManager.bottomLeft.y + radius && direction.y < 0)
      {
          direction.y = -direction.y;
      }
      if (transform.position.y > GameManager.topRight.y - radius && direction.y > 0)
      {
          direction.y = -direction.y;
      }

      //Game Over
      if (transform.position.x < GameManager.bottomLeft.x + radius && direction.x < 0)
      {
          Debug.Log("Right player Wins");
          Time.timeScale = 0;
      }
      if (transform.position.x > GameManager.topRight.x - radius && direction.x > 0)
      {
          Debug.Log("Left player Wins");
          Time.timeScale = 0;
      }
  }
  void OnTriggerEnter2D(Collider2D other)
  {
      if (other.tag == "Paddle")
      {
          bool isRight = other.GetComponent<Paddle>().isRight;
          if (isRight == true && direction.x > 0)
          {
              direction.x = -direction.x;
          }
          if (isRight == false && direction.x < 0)
          {
              direction.x = -direction.x;
          }
      }
  }
}
```
### EXP 04:
#### PLAYER CONTROLLER
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float horizontalInput;
    public float speed = 10.0f;
    public float xRange = 10f;
    public GameObject projectilePrefab;
    // Start is called before the first frame update
    void Start()
    {        
    }
    // Update is called once per frame
    void Update()
    {
        if (transform.position.x < -xRange)
        {
            transform.position = new Vector3(-xRange, transform.position.y, transform.position.z);
        }
        if (transform.position.x > xRange)
        {
            transform.position = new Vector3(xRange, transform.position.y, transform.position.z);

        }
        horizontalInput = Input.GetAxis("Horizontal");
        transform.Translate(Vector3.right * horizontalInput * Time.deltaTime * speed);
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Instantiate(projectilePrefab, transform.position, projectilePrefab.transform.rotation);
        }

    }
}
```
#### MOVE FORWARD:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MoveForward : MonoBehaviour
{
    public float speed = 2.0f;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        transform.Translate(Vector3.forward * Time.deltaTime * speed);
    }
}
```
### EXP 05:
#### Spawn Manager 
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SpawnManager : MonoBehaviour
{
    public GameObject[] animalPrefabs;
    private float spawnRangeX = 20;
    private float spawnPosZ = 20;
    private float startDelay = 2;
    private float spawnInterval = 1.5f;

    // Start is called before the first frame update
    void Start()
    {
        InvokeRepeating("SpawnRandomAnimal", startDelay, spawnInterval);

    }

    // Update is called once per frame
    void Update()
    {

    }
    void SpawnRandomAnimal()
    {
        int animalIndex = Random.Range(0, animalPrefabs.Length);
        Vector3 spawnPos = new Vector3(Random.Range(-spawnRangeX, spawnRangeX), 0, spawnPosZ);
        Instantiate(animalPrefabs[animalIndex], spawnPos, animalPrefabs[animalIndex].transform.rotation);
    }
}
```
#### Detect Collision 
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DetectCollider : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {}

    // Update is called once per frame
    void Update()
    {}
    private void OnTriggerEnter(Collider other)
    {
        Destroy(gameObject);
        Destroy(other.gameObject);
    }
}

```
### EXP 06:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class idleToCrouch : MonoBehaviour
{
    public Animator animator;
    public float InputX;
    public float InputY;
    // Start is called before the first frame update
    void Start()
    {
        animator = this.gameObject.GetComponent<Animator>();
    }

    // Update is called once per frame
    void Update()
    {
        InputY = Input.GetAxis("Vertical");
        InputX = Input.GetAxis("Horizontal");
        animator.SetFloat("InputY",InputY);
        animator.SetFloat("InputX", InputX);
    }
} 
```
### EXP 07:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
public class PlayerController : MonoBehaviour
{
    Rigidbody rb;
    public GameObject WinText;
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            SceneManager.LoadScene("scene2");
        }
    }
    private void OnMouseDown()
    {
        Destroy(gameObject);
    }
    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.tag == "object")
        {
            Destroy(collision.gameObject);
            WinText.SetActive(true);
        }
    }
}
```
