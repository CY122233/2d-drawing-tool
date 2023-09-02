# 2d-drawing-tool
Creating a two-dimensional drawing tool for kids in Unity can be a fun and educational project.
In this tutorial, we'll walk through the basic steps to create a simple drawing tool that allows users to draw lines and shapes on a canvas using their mouse or touch input.

Step 1: Set Up Your Unity Project

1.Open Unity and create a new 2D project.
2.Set up your project by creating a Canvas:
   In the Hierarchy panel, right-click, and choose UI > Canvas.
   Inside the Canvas, create an Image object (right-click Canvas > UI > Image) to act as your drawing area. This image will serve as your canvas.
   Create UI buttons for different colors, line thickness, and a clear button.

Step 2: Create a Script for Drawing

1.Create a new C# script (e.g., "DrawingTool") and attach it to your Canvas object.

2.Open the script and define some variables:
using UnityEngine;
using UnityEngine.UI;
using System.Collections.Generic;

public class DrawingTool : MonoBehaviour
{
    public Color drawColor = Color.black;
    public float lineWidth = 5f;
    public Button clearButton;
    public Image canvasImage;

    private List<Vector2> drawingPoints = new List<Vector2>();
    private bool isDrawing = false;
    private Texture2D canvasTexture;
    private RectTransform canvasRect;
    private Color[] canvasColors;

    void Start()
    {
        canvasRect = canvasImage.rectTransform;
        canvasTexture = new Texture2D((int)canvasRect.rect.width, (int)canvasRect.rect.height);
        canvasImage.sprite = Sprite.Create(canvasTexture, new Rect(0, 0, canvasTexture.width, canvasTexture.height), Vector2.zero);
        canvasColors = new Color[canvasTexture.width * canvasTexture.height];
        clearButton.onClick.AddListener(ClearCanvas);
    }

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            StartDrawing();
        }
        else if (Input.GetMouseButtonUp(0))
        {
            StopDrawing();
        }

        if (isDrawing)
        {
            Vector2 mousePosition = Input.mousePosition;
            Vector2 canvasPos;
            RectTransformUtility.ScreenPointToLocalPointInRectangle(canvasRect, mousePosition, null, out canvasPos);

            drawingPoints.Add(canvasPos);
            DrawLine();
        }
    }

    void StartDrawing()
    {
        isDrawing = true;
        drawingPoints.Clear();
    }

    void StopDrawing()
    {
        isDrawing = false;
    }

    void DrawLine()
    {
        for (int i = 0; i < drawingPoints.Count - 1; i++)
        {
            int startIdx = (int)drawingPoints[i].x + (int)drawingPoints[i].y * canvasTexture.width;
            int endIdx = (int)drawingPoints[i + 1].x + (int)drawingPoints[i + 1].y * canvasTexture.width;

            DrawLineBetweenPoints(startIdx, endIdx);
        }

        canvasTexture.SetPixels(canvasColors);
        canvasTexture.Apply();
    }

    void DrawLineBetweenPoints(int startIdx, int endIdx)
    {
        for (int i = startIdx; i < endIdx; i++)
        {
            canvasColors[i] = drawColor;
        }
    }

    void ClearCanvas()
    {
        canvasColors = new Color[canvasTexture.width * canvasTexture.height];
        canvasTexture.SetPixels(canvasColors);
        canvasTexture.Apply();
    }
}




Step 3: Attach UI Elements

Attach your Canvas object, buttons for different colors, line thickness, and the clear button in the Unity Inspector.


Step 4: Create UI Buttons for Colors and Thickness

You can create UI buttons for different colors and line thickness. Attach these buttons to the script and modify the drawColor and lineWidth properties accordingly when the buttons are clicked.

Step 5: Test and Build

Now, you can test your drawing tool in Unity's Play Mode. Users should be able to draw on the canvas using their mouse or touch input, change colors and line thickness, and clear the canvas when needed.

Once you are satisfied with your drawing tool, you can build and deploy it for your target platform.
