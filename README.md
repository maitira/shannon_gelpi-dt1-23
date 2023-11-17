# shannon_gelpi-dt1-23

## Important details
- Huggingface API Key: API Key: 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
- Model: gpt2 (https://api-inference.huggingface.co/models/gpt2)
- external IP VM: 34.65.165.180
- Port: TCP 5000
- Firewall: Quellfilter: 0.0.0.0/0; Zielfilter: 0.0.0.0/0; Zieltags: "https-server" "http-server"

<img width="709" alt="Screenshot 2023-11-17 at 17 22 46" src="https://github.com/maitira/shannon_gelpi-dt1-23/assets/99893716/1b723965-5bca-40af-93cf-5bda2091ce8f">

Inside the Google Cloud Console is the VM with the Firewall. Docker is locally downloaded on my laptop but can also be dowloaded through the VM. It then connects to Huggingface through the Flask App. Huggingface has contains the model gpt2 which gets the Data from ChatGTP2 and through an API feeds the information to the bubble frontend. The user only sees Bubble and the output the Chatbot generates.

## Backend ##

**Terminal**
1. git clone https://github.com/sid027/dt1-23
2. docker build -t shannon_gelpi-dt1-23:latest
3. docker tag shannon_gelpi-dt1-23 maitira/shannon_gelpi-dt1-23:latest
4. docker push maitira/shannon_gelpi-dt1-23:latest


**VM**
1. sudo apt update
2. sudo apt upgrade
3. sudo usermod -aG docker shannon_maitira
4. sudo apt install docker.io
5. docker login
6. docker pull maitira/shannon_gelpi-dt1-23:latest
7. sudo docker run -p 8080:5000 maitira/shannon_gelpi-dt1-23:latest

Tested if it works in an incognito window "http://34.65.165.180:5000/ping"


## Frontend - BUBBLE ##
Link: https://chatbot-dt1-shannon.bubbleapps.io/version-test?debug_mode=true

(It only works if the person opening the link also allows insecure content)

Final Script:

<script>
  document.getElementById('chat_submit').addEventListener('click', function() {
    // Get user message from input field
    var userMessage = document.getElementById('chat_input').value;
    
    // Call Hugging Face API
    fetch('http://34.65.165.180:5000/chat?model_id=gpt2&huggingface_token=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&input=' + encodeURIComponent(userMessage))
      .then(response => response.json())
      .then(data => {
        // Get output from the API response
        var outputMessage = data.ack;
        
        // Concatenate input and output
        var finalMessage = userMessage + ' ' + outputMessage;
        
        // Display the result in the output element
        document.getElementById('chat_output').innerText = finalMessage;
      })
      .catch(error => console.error('Error:', error));
  });
</script>


### Problems ###

- **Docker Image**: Most embarrassing fail but I am going to list it for your amusement: When I first started with the assignment I just went through the list with ChatGTP without really looking at your GitHub repository. When it said to "build and push the docker image" after having done the rough architecture in Miro, I took a screenshot of the architecture in Miro and try to push it to Docker Hub ^^
- **naming the image**: Image needs to have the same name as the repository I wanted to push it to... 
- **5000 vs. 8080**: Another thing I struggled with until the very end was to figure out which port I was using and switched back and forth between 8080 and 5000... running both was possible on VM but 8080 seemed to not work with bubble. I just tried everything until it worked but I'm still not exactly sure why. I know the Flask App has port 5000 so i adapted to that but initially I think it was 8080.
- **amd64 vs. arm64**: The biggest and most time-consuming problem for me was the error message:

    "WARNING: The requested image's platform (linux/arm64/v8) does not match the detected host platform (linux/amd64) and no specific platform was requested
    standard_init_linux.go:219: exec user process caused: exec format error"

    I had to build and push the image to docker again while changing the settings to match the VM (since I tried but couldn't change it there). After there was a warning in Docker that
    it might not run properly or fail but it seems to work, so..

- **. & "&input"**: At the start it didn't work at all building the image until I realized I forgot the full stop at the end. Same with the Bubble HTML code where I deleted the "&input" after the API token and went to desperation hole for a minute.
- **Running the Image in the VM**: it never finished running the code but it worked anyways, I just closed the SSH window (might still be running)

  docker run -p 8080:5000 maitira/shannon_gelpi-dt1-23
     * Serving Flask app 'main'
     * Debug mode: on
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
     * Running on all addresses (0.0.0.0)
     * Running on http://127.0.0.1:5000
     * Running on http://172.17.0.2:5000
    Press CTRL+C to quit
     * Restarting with stat
     * Debugger is active!
     * Debugger PIN: 554-520-027

- **allow mixed content**: Even though I think you mentioned it in Teams, until today my Bubble Chatbot wasn't working. My classmate reminded me to allow "insecure content" in Google Chrome settings...
- **add to group**: While downloading docker on the VM I got the error "docker not found" or something, asked ChatGTP and had to "add my user to the group". After repeatedly asking more questions, I had added my VM user to some docker group and after it was working.  Apparently the "sudo" command does the same thing (tip from a classmate)
