# shannon_gelpi-dt1-23

Important details
-Huggingface API Key: API Key: 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
-Model: gpt2 (https://api-inference.huggingface.co/models/gpt2)
-external IP VM: 34.65.165.180/>
-Firewall: Quellfilter: 0.0.0.0/0; Zielfilter: 0.0.0.0/0; Zieltags: "https-server" "http-server"

<img width="741" alt="Screenshot 2023-11-17 at 16 49 37" src="https://github.com/maitira/shannon_gelpi-dt1-23/assets/99893716/b5c39b7c-64d1-4d9c-a213-c66aae2bc1b5">


**BUBBLE**
Link: https://chatbot-dt1-shannon.bubbleapps.io/version-test?debug_mode=true

I don't know if it works for you, when I sent it to another computer it didn't work anymore.. 

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


**DOCKER**





**Problems**

most embarrassing fail: I thouhgt initially docker image ^^
5000 vs. 8080
amd64 vs. arm64
ubuntu vs. debian
. 
allow mixed content
add to group docker / sudo (classmate)
