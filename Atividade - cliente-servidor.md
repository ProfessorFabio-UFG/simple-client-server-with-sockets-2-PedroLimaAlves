Pedro Henrique Lima – 202206799

Funcionalidade: calculadora remota de soma, subtração, multiplicação e divisão
Descrição: 
Parte servidor:
O servidor é configurado para usar o protocolo TCP na porta 5678 (que foi a indicada no Security Group em sala de aula pelo professor). 
O servidor aguarda por conexões com o comando accept(), que cria um novo socket para cada cliente que se conecta.
Quando o cliente envia dados (como valores e a operação desejada), o servidor os processa, realiza a operação e envia o resultado de volta ao cliente.
Parte cliente:
O cliente utiliza o protocolo TCP para se conectar ao servidor, enviar dados para execução de operações e receber os resultados.
O cliente envia dados ao servidor, como dois números e o tipo de operação que deseja realizar (ex: adicionar, subtrair).
O cliente recebe a resposta do servidor através da conexão TCP e exibe o resultado da operação no terminal.
Após receber a resposta, o cliente encerra a conexão TCP com o servidor.



Código Servidor – servidor.py

    import socket
    
    def handle_client(conn):
        data = conn.recv(1024).decode()
        if not data:
            return
        try:
            op, num1, num2 = data.split()
            num1, num2 = float(num1), float(num2)

        if op == "add":
            result = num1 + num2
        elif op == "subtract":
            result = num1 - num2
        elif op == "multiply":
            result = num1 * num2
        elif op == "divide":
            result = num1 / num2 if num2 != 0 else "Erro: Divisão por zero"
        else:
            result = "Erro: Operação inválida"

        conn.send(str(result).encode())

    except Exception as e:
        conn.send(f"Erro: {e}".encode())

    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.bind(("0.0.0.0", 5678))
    server.listen(1)
    print("Servidor aguardando conexões...")
    
    while True:
        conn, addr = server.accept()
        handle_client(conn)
        conn.close()
Código Cliente – cliente.py
        
        import socket
        
        server_ip = input("Digite o IP do servidor: ")
        client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        client.connect((server_ip, 5678))
        
        op = input("Digite a operação (add, subtract, multiply, divide): ")
        num1 = input("Digite o primeiro número: ")
        num2 = input("Digite o segundo número: ")
        
        client.send(f"{op} {num1} {num2}".encode())
        
        result = client.recv(1024).decode()
        print("Resultado:", result)
        
        client.close()
