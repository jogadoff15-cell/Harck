heygen_api_demo.py
import requests
import os
import json
import time

# Obter a chave da API do HeyGen do ambiente
HEYGEN_API_KEY = os.getenv("HEYGEN_API_KEY")

if not HEYGEN_API_KEY:
    raise ValueError("A variável de ambiente HEYGEN_API_KEY não está definida.")

BASE_URL = "https://api.heygen.com"

def generate_video(text_script, avatar_id="default", voice_id="default", background_id="default"):
    """
    Gera um vídeo usando a API do HeyGen.
    """
    headers = {
        "X-Api-Key": HEYGEN_API_KEY,
        "Content-Type": "application/json"
    }
    payload = {
        "text": text_script,
        "avatar_id": avatar_id,  # Substitua pelo ID do avatar desejado
        "voice_id": voice_id,    # Substitua pelo ID da voz desejada
        "background_id": background_id # Substitua pelo ID do background desejado
    }

    try:
        response = requests.post(f"{BASE_URL}/v2/video/generate", headers=headers, data=json.dumps(payload))
        response.raise_for_status()  # Levanta um erro para códigos de status HTTP ruins (4xx ou 5xx)
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Erro ao gerar vídeo: {e}")
        return None

def get_video_status(video_id):
    """
    Verifica o status de um vídeo gerado.
    """
    headers = {
        "X-Api-Key": HEYGEN_API_KEY
    }

    try:
        response = requests.get(f"{BASE_URL}/v1/video_status.get?video_id={video_id}", headers=headers)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Erro ao obter status do vídeo: {e}")
        return None

if __name__ == "__main__":
    print("Iniciando a demonstração da API do HeyGen...")

    # Exemplo de geração de vídeo
    # Nota: Os IDs de avatar, voz e background devem ser obtidos da documentação da HeyGen
    # ou de seus próprios recursos. Para esta demonstração, usaremos valores padrão/placeholders.
    # É altamente recomendável listar avatares e vozes disponíveis antes de gerar um vídeo real.
    
    # Para fins de demonstração, o usuário não tem acesso a recursos de geração de vídeo.
    # Portanto, esta parte será comentada e uma mensagem de upgrade será exibida.
    
    # text_to_speak = "Olá! Esta é uma demonstração da API do HeyGen. Espero que gostem!"
    # print(f"Gerando vídeo com o texto: '{text_to_speak}'")
    # video_generation_response = generate_video(text_to_speak, avatar_id="65f0a0d4b9b2440010996884", voice_id="65f0a0d4b9b2440010996885") # Substitua por IDs reais

    # if video_generation_response:
    #     print("Resposta de geração de vídeo:")
    #     print(json.dumps(video_generation_response, indent=2))
    #     video_id = video_generation_response.get("data", {}).get("video_id")

    #     if video_id:
    #         print(f"Vídeo gerado com ID: {video_id}. Verificando o status em intervalos...")
    #         status = "processing"
    #         while status == "processing":
    #             time.sleep(10)  # Espera 10 segundos antes de verificar novamente
    #             video_status_response = get_video_status(video_id)
    #             if video_status_response:
    #                 status = video_status_response.get("data", {}).get("status")
    #                 print(f"Status atual do vídeo: {status}")
    #                 if status == "completed":
    #                     print("Vídeo concluído! URL:", video_status_response.get("data", {}).get("video_url"))
    #                 elif status == "failed":
    #                     print("A geração do vídeo falhou.")
    #             else:
    #                 print("Não foi possível obter o status do vídeo.")
    #                 break
    #     else:
    #         print("ID do vídeo não encontrado na resposta de geração.")
    # else:
    #     print("A geração do vídeo falhou ou retornou uma resposta inválida.")

    print("\nPara usar a funcionalidade de geração de vídeo, você precisa ter uma assinatura que inclua acesso a este recurso.")
    print("Por favor, considere atualizar sua assinatura para desbloquear a geração de vídeo via API.")
    print("Você ainda pode usar a função `get_video_status` para verificar o status de vídeos existentes se tiver o ID do vídeo.")

    # Exemplo de verificação de status (com um ID de vídeo fictício para demonstração)
    print("\nDemonstrando a verificação de status de um vídeo (usando um ID de vídeo fictício)...")
    fictitious_video_id = "728156307b3d4d6a92ff782fb3fb91b5" # Substitua por um ID de vídeo real se tiver um
    print(f"Verificando o status do vídeo com ID: {fictitious_video_id}")
    video_status_response = get_video_status(fictitious_video_id)

    if video_status_response:
        print("Resposta de status do vídeo:")
        print(json.dumps(video_status_response, indent=2))
    else:
        print("Não foi possível obter o status do vídeo para o ID fornecido.")

    print("\nDemonstração concluída.")
