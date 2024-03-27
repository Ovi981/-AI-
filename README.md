# -AI-
味觉探索应用生成式 AI 和大语言模型,根据用户的口味偏好,生成个性化的菜谱和美食推荐,为用户带来全新的烹饪灵感和味蕾享受。
import openai
from flask import Flask, request, jsonify

app = Flask(__name__)

# Assuming you have an API key from OpenAI or any other generative AI service provider
openai.api_key = 'your_api_key_here'

def generate_food_recommendations(preferences):
    """
    Generates personalized food recommendations based on user preferences.
    
    :param preferences: A string containing user's food preferences
    :return: A string with generated food recommendations
    """
    try:
        response = openai.Completion.create(
          engine="text-davinci-003", # You might need to update the engine name based on the available options
          prompt=f"Given the food preferences: {preferences}. Generate a personalized recipe and food recommendations.",
          temperature=0.7,
          max_tokens=150,
          top_p=1.0,
          frequency_penalty=0.5,
          presence_penalty=0.0
        )
        return response.choices[0].text.strip()
    except Exception as e:
        return f"Error generating recommendations: {str(e)}"

@app.route('/get_recommendations', methods=['POST'])
def get_recommendations():
    """
    API endpoint to get personalized food recommendations.
    Expects a JSON payload with user preferences.
    """
    data = request.json
    preferences = data.get('preferences', '')
    
    if not preferences:
        return jsonify({'error': 'Preferences not provided'}), 400
    
    recommendations = generate_food_recommendations(preferences)
    return jsonify({'recommendations': recommendations})

if __name__ == '__main__':
    app.run(debug=True, port=5000)
