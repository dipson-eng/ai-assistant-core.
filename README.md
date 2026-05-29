from flask import Flask, request, jsonify

app = Flask(__name__)

def ai_response(message):

    message = message.lower()

    if "force" in message:
        return "Force is a push or pull that changes motion of an object."

    if "energy" in message:
        return "Energy is the ability to do work."

    if "hello" in message:
        return "Hello! I am your AI tutor."

    return "Ask me Physics, Math, or Chemistry questions."


@app.route("/")
def home():

    return """
    <!doctype html>

    <html>

    <head>
        <title>AI Tutor</title>
    </head>

    <body>

        <h2>AI Tutor Chat</h2>

        <div id="chat"></div>

        <br>

        <input id="msg" placeholder="Ask something..." />

        <button onclick="send()">Send</button>

        <script>

        function send() {

            let message = document.getElementById("msg").value;

            if (!message) {
                return;
            }

            fetch("/chat", {

                method: "POST",

                headers: {
                    "Content-Type": "application/json"
                },

                body: JSON.stringify({
                    message: message
                })

            })

            .then(res => res.json())

            .then(data => {

                document.getElementById("chat").innerHTML +=
                "<b>You:</b> " + message + "<br>" +
                "<b>AI:</b> " + data.answer + "<br><br>";

                document.getElementById("msg").value = "";

            });

        }

        </script>

    </body>

    </html>
    """


@app.route("/chat", methods=["POST"])
def chat():

    data = request.get_json()

    message = data.get("message")

    reply = ai_response(message)

    return jsonify({
        "question": message,
        "answer": reply
    })


if __name__ == "__main__":

    app.run(debug=True)
