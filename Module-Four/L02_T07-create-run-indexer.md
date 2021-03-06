# Exercise - Create and run an indexer for Margie's Travel

Select your preferred language below, and then follow the steps below to create and run an indexer for the Margie's Travel search solution.

### Using Python

To create an indexer with Python, you need to submit a request to the **indexers** REST endpoint with the name of the indexer. The body of the request must be a JSON document that defines the indexer.

1. In the **Python/create-enriched-index** folder, open the **indexer.json** file. This file contains the JSON definition of an indexer.
2. Review the indexer definition and observe the following:
    - The indexer maps fields from the **margies-docs-py** data source to the **margies-index-py** index, and uses the **margies-skillset-py** skillset to generate enriched fields.
    - The **parameters** configuration includes an imageAction setting with the value **generateNormalizedImages** - this causes the indexer to extract images from the source data and put the extracted image data in a **normalized_images** collection field in the **document**.
    - The **fieldMappings** list maps file metadata extracted directly from the data source to index fields. This represents the initial state of the document that will be processed by the pipeline.
    - The **outputMappings** list maps the enriched data values generated by the skillset to index fields after the pipeline has finished.
    - The **imageDescription** output from the image analysis skill is mapped to the **image_description** index field. Recall that this field is a complex object consisting of a collection of tags and a collection of captions.
    - The **text** values for the captions in the description are mapped directly to the **image_caption** index field. This demonstrates that you can choose to map an entire complex object generated by a skill to a field, or map subelements of the object to individual fields.
    - The **mergedText** value, which combines the original text content and the text extracted from images, is mapped to the **content** image field.
3. In the **Terminal** pane, select the bash terminal for the **Python/create-enriched-index** folder. If you have closed this terminal, right-click (CTRL+click if using a Mac) the **create-enriched-index** folder and select **Open in Integrated Terminal**.
4. In the terminal for the **create-enriched-index** folder, enter the following command:
    ```bash
    python3 submit-rest.py 'PUT' 'indexers/margies-indexer-py' 'indexer.json'
    ```
5. Wait while Python runs the **submit-rest&#46;py** script, causing it to submit an HTTP PUT request to the *indexers* REST endpoint, adding an indexer named *margies-indexer-py* based on the JSON body defined in the *indexer.json* file. The use of a PUT request ensures that if the indexer already exists, it is updated based on the JSON; otherwise it is created.
6. Review the JSON response that is returned from the REST interface. The indexer is created and automatically run to initialize the index.
7. In the terminal for the **create-enriched-index** folder, enter the following command:
    ```bash
    python3 submit-rest.py 'GET' 'indexers/margies-indexer-py/status' 'null'
    ```
8. Review the JSON response that is returned from the REST interface, which shows the status of the indexer. In particular, check the **status** value in the **lastResult** section of the response. If this is shown as **inProgress**, the indexer is still being applied to the index. You can rerun the previous command to retrieve the status until the last result status is **success**.
9. Open your search service in the [Azure portal](https://portal.azure.com?portal=true) and view its **Indexers** tab to confirm that the indexer has been created and has processed 72 documents.

    > [!NOTE]
    > There may be some warnings indicating that some documents were too large for the sentiment skill to analyze fully, and only the first 1000 characters of these documents were processed. You can ignore these warnings.

### Using Csharp

To create an indexer, you can use the **Create** method of the **SearchServiceClient**'s **Indexers** member. When you initially create an indexer, it runs to populate the index. To check the status of an indexer, you use the **GetStatus** method of the **SearchServiceClient**'s **Indexers** member.

1. In the **C-Sharp/create-enriched-index** folder, open the **Program.cs** file and review the code in the **CreateIndexer** function, observing that:
    - The indexer configuration includes an **imageAction** setting with the value **generateNormalizedImages** - this causes the indexer to extract images from the source data and put the extracted image data in a **normalized_images** collection field in the **document**.
    - The **fieldMappings** list maps file metadata extracted directly from the data source to index fields. This represents the initial state of the document that will be processed by the pipeline.
    - The **outputMappings** list maps the enriched data values generated by the skillset to index fields after the pipeline has finished.
    - The **imageDescription** output from the image analysis skill is mapped to the **image_descriptions** index field. Recall that this field is a complex object consisting of a collection of tags and a collection of captions for each image in the document.
    - The **text** values for the captions in the description are mapped directly to the **image_captions** index field. This demonstrates that you can choose to map an entire complex object generated by a skill to a field, or map subelements of the object to individual fields.
    - The **mergedText** value, which combines the original text content and the text extracted from images, is mapped to the **content** image field.
2. Review the code in the **CheckIndexerOverallStatus** function, which retrieves the indexer status.
3. In the **Terminal** pane, select the bash terminal for the **create-enriched-index** folder. If you have closed this terminal, right-click (Ctrl+click if using a Mac) the **C-Sharp/create-enriched-index** folder and select **Open in Integrated Terminal**.
4. In the terminal for the **create-enriched-index** folder, enter the following command:
    ```bash
    dotnet run
    ```
5. When prompted, press **4** to create and run an indexer. Then wait while the program creates the indexer and then retrieves it status.
6. When the prompt is redisplayed, press **q** to quit the program.
7. Open your search service in the [Azure portal](https://portal.azure.com?portal=true) and view its **Indexers** tab to confirm that the indexer has been created and has processed 72 documents.

    > [!NOTE]
    > There may be some warnings indicating that some documents were too large for the sentiment skill to analyze fully, and only the first 1000 characters of these documents were processed. You can ignore these warnings.
