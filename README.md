Project _ Motion Classification

Introduction
This is the practice project of Introduction to Embedded Machine Learning from Edge Impulse. 
Edge Impulse provides a platform to collect data from your hardware, train the model, deploy the trained model onto your hardware, and do the inference on the edge. 

The project includes collecting data, performing feature extraction, training the model, and deploying the trained model onto the hardware. 

This project will infer the different types of surfaces through an accelerometer on the Thingy53 attached to a robot cleaner. 

Preparation
- Hardware: Thingy53
- Platform: Edge Impulse

- Setup:
      * robot cleaner
      * Five Surfaces: Wood/ Concrete/ Obstacles/ Thick-carpet/ Thin-carpet
      * Thingy53
  ![five_surfaces](https://github.com/user-attachments/assets/20f85ffa-d009-4aeb-879d-bc77da6b1626)

Test Procedure: 
1. Update Thingy53 with the Edge Impulse firmware. (link: https://docs.edgeimpulse.com/docs/edge-ai-hardware/mcu/nordic-semi-thingy53)
2. Connect Thingy53 to Edge Impulse
![image](https://github.com/user-attachments/assets/63a65567-bdb6-4a5a-9bbc-07ee614b52cc)

3. Attach Thingy53 on the top of the Robot Cleaner
   ![image](https://github.com/user-attachments/assets/717cbe5c-9114-4c0c-b372-b96383dae5a3)
use tape to fix the Thingy53 on top of the Robot Cleaner
4. Data Collection
   Using the nRFEdgeImpulse to connect Thingy53.
   Then go to the Data Acquisition, click the "+", choose "Accelerometer" from the Sensor list.
   Put the Label name. Here, we have five different surface_ Wood/ Concrete/ Obstacles/ Thick-carpet/ Thin-carpet
   Time:
     - Training: 110s
     - Testing: 30s
  
   Start the Robot Cleaner, then click "Start Sampling" on the nRFEdgeImpulse App.
   Data Collection Scheme:
     * Training:
         - Wood: 110 seconds (x2 times)
         - Concrete: 110 seconds (x2 times)
         - Obstacles: 110 seconds (x2 times)
         - Thick-carpet: 110 seconds (x2 times)
         - Thin-carpet: 110 seconds (x2 times)
     * Testing:
         - Wood: 30 seconds (x2 times)
         - Concrete: 30 seconds (x2 times)
         - Obstacles: 30 seconds (x2 times)
         - Thick-carpet: 30 seconds (x2 times)
         - Thin-carpet: 30 seconds (x2 times)
      
    After that, you can find all the data in the Edge Impulse website in your account.
![image](https://github.com/user-attachments/assets/88fb146d-1269-436d-83f1-d5255d308586)

  5. Create an "Impulse":
     * insert "Spectral Analysis"
     * Add ML mode: "Classification" and "Anomaly Detection (K-means)"
    
       1) "Spectral features"
          - Analysis Type: FFT (length: 16)
          - Generate features
      ![image](https://github.com/user-attachments/assets/9f08c76d-6f53-4715-90cc-57a6a4a40955)

       2) Classifier
          Keep the Neural Network settings.
          Start training
      ![image](https://github.com/user-attachments/assets/968ff04f-5490-4f83-92bf-962d62e12171)

      ![image](https://github.com/user-attachments/assets/8a5d6ed9-4a14-4456-b81c-f9530d51d059)

        3) Anomaly detection
           check the main three axis: accX RMS/ accY RMS/ accZ RMS
           Start training
![image](https://github.com/user-attachments/assets/df9478ce-f3cb-4ddc-9ba9-db4c2ccc5be0)

        4) Model testing
        click "Classify all"
![image](https://github.com/user-attachments/assets/13cec261-f4dd-4a29-8c9c-c63ab3fb16ba)

      Result: 
      * Accuracy: 63.69% 
     ![image](https://github.com/user-attachments/assets/c01115f3-aeb8-478b-948d-461bfc972f3e)
      
      * Confusion matrix
     ![image](https://github.com/user-attachments/assets/17140e95-75d1-4ce2-ae04-8b028b6a59d0)

      * Feature explorer
     ![image](https://github.com/user-attachments/assets/450e4dc4-5f8d-4271-b217-12403e8356db)

        5) Deployment
    Choose the "EON Compiler" then click "build" to generate the binary firmware file for Thingy53

  6. Program Thingy53 with the new Edge Impulse firmware
     Program Thingy53 with the new .zip EdgeImpulse firmware through USB-cable in the nRF Programmer in the MCUboot DFU mode. (link: https://docs.nordicsemi.com/bundle/nrf-connect-programmer/page/programming_dk.html#programming-nordic-thingy53)

  7. Use the 'nRF EdgeImpulse' for Inferencing.
     - Open 'nRF EdgeImpulse' App, connect Thingy53.
     - Put the Thingy53 on the Robot Cleaner, start Robot Cleaner on different surfaces.
     - Go to the "Inferencing" tab in App, click start
       Results:
       * Wood:
       ![image](https://github.com/user-attachments/assets/826a77a4-ee5e-48fc-86e6-5f6d2c9439ab)

       * Concrete:
       ![image](https://github.com/user-attachments/assets/b6d11c05-9255-46d4-b742-0cc185ed918a)

       * Obstacles:
       ![image](https://github.com/user-attachments/assets/81401a42-e9fc-44dc-a1d8-8273efecf834)

       * Thick-carpet:
       ![image](https://github.com/user-attachments/assets/441c4f59-85dd-412f-bdb9-e458b98037cc)

       * Thin-carpet:
       ![image](https://github.com/user-attachments/assets/dda31038-a7f1-46b1-876d-5f7ff3b8f440)


Conclusion: 
From the inference results, we can see that the "Thin-carpet" is the most distinguishable class. 
In the "Obstacles" class, the accuracy is also high. There are two Anormaly in the "Obstacles" class, which were caused by the moment that the Robot Cleaner knocked the obstacle and generated the abnormal behaviors. 
