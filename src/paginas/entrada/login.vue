<template>
    <div class="login">
        <img src="../../assets/logo-ext-roxo.png" alt="TecnoMinds">

        <h1 class="titulo">É bom te ver de volta!</h1>
        <h5 class="titulo2">Faça Login e acesse nosso sistema</h5>

        <!-- <form action="/home"> -->
        <form>
            <input type="text" name="usuario" id="usuario" placeholder="Usuário ou E-mail" v-model="usuario">
            <input type="password" name="senha" id="senha" placeholder="Senha" v-model="senha">

            <button type="submit" @click.prevent="realizarLogin">Entrar</button>
            <p>Ainda não possui conta? <RouterLink to="/cadastro">Crie uma</RouterLink>!</p>{{ usuario }}
        </form>
    </div>
</template>

<script setup>
import router  from '../../rotas/'
import { ref } from 'vue'

const usuario = ref("");
const senha = ref("");

function realizarLogin() {
    const data = {email: usuario.value, senha: senha.value}

    fetch('http://localhost:3000/auth/login', {
            method: 'POST',
            //credentials: 'include', 
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': 'http://localhost:5173', // Adiciona o cabeçalho indicando o tipo de conteúdo
            },
            body: JSON.stringify(data) // Converte os dados para JSON
        })
        .then(response => response.json()) // Converte a resposta para JSON
        .then(data => {
console.log(data)
            if (data.token) {
                router.push("/home")
                console.log(data);
                localStorage.token = data.token;
                localStorage.nome_usuario = usuario.value
            } else {
                alert("senha errada")
            }
            /*if (data.msg === "Login realizado com sucesso!") {
                // Se o login for bem-sucedido, redireciona para a página principal
                window.location.href = "home.html";
            } else {
                // Exibe uma mensagem de erro
                alert("Erro ao realizar login: " + data.msg);
            }*/
        })
        .catch(error => {
            // Exibe um erro em caso de falha na requisição
            alert("Erro na requisição: " + error.message);
        });
}
</script>


<style lang="scss">
.login {
    margin-top: 5vh;
    display: flex;
    flex-direction: column;

    img {
        width: 200px;
        height: 40px;
        margin-inline: auto;
        margin-bottom: 5vh;
    }

    h1, h5 {
        margin: 0;
        text-align: center;
    }

    form {
        width: 25%;
        margin-inline: auto;
        display: flex;
        flex-direction: column;
        padding: 30px 20px;

        input {
            padding: 10px;
            outline: none;
            border: 1px solid #777;
            border-radius: 5px;
            margin-bottom: 20px;
        }

        button {
            outline: none;
            padding: 10px 5px;
            background-color: #990EF1;
            border: none;
            color: #fff;
            font-weight: 600;
            border-radius: 5px;
            text-transform: uppercase;
        }

        p {
            margin-top: 10px;
            text-align: center;

            a {
                text-decoration: none;
                color: #990EF1;
                font-weight: 500;
            }
        }
    }
}

@media (max-width: 576px) {
    .login {
        width: 80vw;
        margin-inline: auto;

        h1 {
            font-size: 25px;
            font-weight: 700;
        }
        form {
            width: 90%;
        }
    }
}
</style>