---
aliases:
  - Canvas
  - Rendering Mode del Canvas
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: 
cssclasses:
  - 
---
## Resettare una scena con un pulsante
Basta ricaricare la scena attuale.

```CS
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

// Applicarlo su un GameObject vuoto
public class ResetScene : MonoBehaviour
{
    public Button button;
    
    // Start is called before the first frame update
    void Start()
    {
        button.onClick.AddListener(Task);
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    void Task()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}
```

## Colore a caso

```CS
Renderer renderer = objectToChange.GetComponent<Renderer>(); 
// Oppure gameObject per l'oggetto su cui è applicato lo script

if (renderer != null)
{
    Color randomColor = new Color(Random.value, Random.value, Random.value);
    renderer.material.color = randomColor;
}

// Oppure fai selezionare un colore specifico dall'inspector e applicalo (public Color c)
```

## Aggiungi al punteggio e disabilita

```CS
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

// Applicare sul player
public class AddToScore : MonoBehaviour
{
    public Text scoreText;
    private int score = 0;

    void Start()
    {
        scoreText.text = $"Score: {score}";
    }

    void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.CompareTag("MyTag"))
        {
            score++;
            scoreText.text = $"Score: {score}";
            other.gameObject.SetActive(false);
        }
    }
}
```

## Camera Follower

```CS
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// Applicare sulla telecamera
public class CameraFollow : MonoBehaviour
{
    public GameObject objectToFollow;
    public Vector3 offset = new Vector3(0, 6, -12);

    void LateUpdate()
    {
        transform.position = objectToFollow.transform.position + offset;
    }
}
```

## AddForce

### Frecce

```CS
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// Applicare all'oggetto da spostare
public class MoveObject : MonoBehaviour
{
    private Rigidbody rb;
    public float movementSpeed = 5f;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");

        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);

        rb.AddForce(movementSpeed * movement);
    }
}
```

### Frecce con salto
```CS
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// Applicare all'oggetto da spostare
public class MoveObject : MonoBehaviour
{
    private Rigidbody rb;

    public float movementSpeed = 5f;
    public float jumpSpeed = 200f;
    private bool isGrounded;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");

        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
        rb.AddForce(movementSpeed * movement);

        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            Vector3 jump = new Vector3(0.0f, jumpSpeed, 0.0f);
            rb.AddForce(movementSpeed * jump);
        }

    }

    private void OnCollisionExit(Collision collision)
    {
        if (collision.gameObject.CompareTag("Platforms"))
        {
            isGrounded = false;
        }
    }

    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("Platforms"))
        {
            isGrounded = true;
        }
    }
}
```

## Translate

```CS
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// Applicare all'oggetto da spostare
public class MoveObject : MonoBehaviour
{
    public float movementSpeed = 5f;

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if(Input.GetKey(KeyCode.LeftArrow))
        {
            transform.Translate(movementSpeed * Time.deltaTime * Vector3.left);
        }
        if(Input.GetKey(KeyCode.RightArrow))
        {
            transform.Translate(movementSpeed * Time.deltaTime * Vector3.right);
        }
    }
}
```

---
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]