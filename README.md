<h1>Adicionar Campo Novo Para cadastro de Clientes</h1>
Nesse repositório estarei documentando como criar um novo campo de cadastro para customers(clientes).<br>

<h4>Criar um módulo.</h4>
Primeiro será necessário criarmos um módulo, caso não tenha conhecimento de como criar um módulo e suas configurações basicas, basta <a href="https://github.com/ElNogara/Primeiro-Modulo-Magento-1">clicar aqui</a> que você aprende bem rapidinho, só não vamos utilizar o controller criado nesse módulo então pode retirar o arquivo do controller.


<h3>Vamos começar!</h3>

Estou criando o módulo <strong>Elnogara_Newfield</strong> para utilizarmos como exemplo aqui, primeiramente vamos precisar configurar o nosso <strong>config.xml</strong> para declararmos nosso arquivo de setup, que é responsável por conectar no banco de dados do Magento e executar alguma query.

```
<?xml version="1.0"?>
<config>
    <modules>
        <Elnogara_Newfield>
            <version>1.0.0</version>
        </Elnogara_Newfield>
    </modules>
    <global>
        <resources>
            <elnogara_newfield_setup> //Esse nome deve ser o mesmo da pasta criada para realizar a conexão com o banco de dados.
                <setup>
                    <module>Elnogara_Newfield</module> //Nome do seu módulo
                    <class>Mage_Customer_Model_Resource_Setup</class> //Essa classe deve ser declarada para funcionar corretamente a conexão com o banco.
                </setup>
            </elnogara_newfield_setup>
        </resources>
    </global>
</config>
```

Além do arquivo de configuração do módulo vamos precisar de um arquivo de setup, então dentro da raiz do seu módulo crie a pasta "sql", e dentro dessa pasta tera de criar outra pasta com o mesmo nome da tag criada anteriormente, no meu caso é elnogara_newfield_setup e então dentro dessa pasta devemos criar o arquivo install-1.0.0.php que é responsável por executar as querys no banco de dados.
Dentro desse arquivo vamos deixar as seguintes configurações:

```
<?php
$this->addAttribute('customer', 'novocampo', array( //O nome do campo 'novocampo' deve ser alterado para o nome do campo que você quiser utilizar.
    'type'      => 'varchar',
    'label'     => 'Novo Campo', //Esse campo é responsável pelo nome de exibição do campo.
    'input'     => 'text',
    'position'  => 120,
    'required'  => false,
    'is_system' => 0,
));
$attribute = Mage::getSingleton('eav/config')->getAttribute('customer', 'novocampo'); //Aqui também deve ser alterado.
$attribute->setData('used_in_forms', array(
    'adminhtml_customer',
    'checkout_register',
    'customer_account_create',
    'customer_account_edit',
));

$attribute->setData('is_user_defined', 0);
$attribute->save();
```
Essas linhas de códigos são responsáveis por executar a query e criar o "novocampo" dentro do banco de dados.

Para finalizarmos é necessário localizar seu arquivo responsável pelo formulário da pagina de cadastro. Tente localizar o arquivo em app/design/frontend/base/default/template/persistent/customer/form/register.phtml com o arquivo localizado insira o código abaixo dentro do formulário de cadastro:
```
<li>
    <label for="novocampo"><?php echo $this->__('Novo Campo') ?></label> // Não se esquece de trocar todos os campos "novocampo" para o nome que você escolheu no seu arquivo de SQL.
    <div class="input-box">
        <input type="text" name="novocampo" id="novocampo" value="<?php echo $this->escapeHtml($this->getFormData()->getLicenseNumber()) ?>" title="<?php echo $this->__('Novo campo criado') ?>" class="input-text" />
    </div>
</li>
```

<strong>Espero muito ter ajudado. Mas qualquer dúvida estou a disposição - <a href="https://wellingtonnogara.com/" style="color: red;">Wellington Nogara</a>.</strong>
