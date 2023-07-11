# Desenvolvimento de uma API em PHP utilizando o Framework Laravel

### Configuração do ambiente
Para começar, é necessário ter o ambiente de desenvolvimento configurado. Isso inclui ter o PHP instalado, bem como o Composer, um gerenciador de dependências do PHP. Além disso, precisamos instalar o Laravel Framework. O Laravel pode ser instalado globalmente ou por projeto usando o Composer.

### Criação do projeto Laravel
Com o ambiente devidamente configurado, utilizamos o comando ```composer create-project --prefer-dist laravel/laravel nome-do-projeto``` para criar um novo projeto Laravel. Isso criará uma estrutura inicial para o nosso projeto, incluindo as pastas e arquivos necessários.

### Definição das rotas
No Laravel, as rotas são definidas no arquivo ```routes/api.php```. Neste arquivo, especificamos os endpoints disponíveis e os controladores associados a cada rota. Podemos definir rotas para a criação, leitura, atualização e exclusão de tarefas.

### Definição Utilizada:
api.php
```
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/tasks', 'TaskController@index');
Route::get('/tasks/{id}', 'TaskController@show');
Route::post('/tasks', 'TaskController@store');
Route::put('/tasks/{id}', 'TaskController@update');
Route::delete('/tasks/{id}', 'TaskController@destroy');
```

web.php
```
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\TaskController;

Route::get('/tasks', [TaskController::class, 'index']);
Route::get('/tasks/{id}', [TaskController::class, 'show']);
Route::post('/tasks', [TaskController::class, 'store']);
Route::put('/tasks/{id}', [TaskController::class, 'update']);
Route::delete('/tasks/{id}', [TaskController::class, 'destroy']);
```

### Criação dos controladores
Os controladores são responsáveis por tratar as requisições recebidas pelas rotas e retornar as respostas adequadas. Podemos criar um controlador para lidar com as operações CRUD (Create, Read, Update, Delete) relacionadas às tarefas. Cada método dentro do controlador corresponderá a uma operação específica, como criar uma nova tarefa, recuperar uma tarefa existente, atualizar uma tarefa ou excluí-la.

TaskController.php
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Task;

class TaskController extends Controller
{
   public function index()
   {
       $tasks = Task::all();

       return response()->json($tasks);
   }

   public function show($id)
   {
       $task = Task::find($id);

       if ($task) {
           return response()->json($task);
       } else {
           return response()->json(['error' => 'Tarefa não encontrada'], 404);
       }
   }

   public function store(Request $request)
   {
       $request->validate([
           'title' => 'required',
           'description' => 'required',
       ]);

       $task = new Task;
       $task->title = $request->input('title');
       $task->description = $request->input('description');
       $task->status = $request->input('status', false);
       $task->save();

       return response()->json($task, 201);
   }

   public function update(Request $request, $id)
   {
       $task = Task::find($id);

       if ($task) {
           $request->validate([
               'title' => 'required',
               'description' => 'required',
           ]);

           $task->title = $request->input('title');
           $task->description = $request->input('description');
           $task->status = $request->input('status', $task->status);
           $task->save();

           return response()->json($task);
       } else {
           return response()->json(['error' => 'Tarefa não encontrada'], 404);
       }
   }

   public function destroy($id)
   {
       $task = Task::find($id);

       if ($task) {
           $task->delete();

           return response()->json(['message' => 'Tarefa excluída com sucesso']);
       } else {
           return response()->json(['error' => 'Tarefa não encontrada'], 404);
       }
   }
}
```

### Implementação dos modelos e migrações
No Laravel, utilizamos os modelos para representar as entidades do nosso sistema, nesse caso, a entidade "Task". Podemos criar um modelo para a tarefa utilizando o comando ```"php artisan make:model Task```. Além disso, o Laravel oferece as migrações, que são responsáveis por criar as tabelas no banco de dados. Podemos criar uma migração para a tabela de tarefas utilizando o comando ```php artisan make:migration create_tarefas_table --create=task```.

2023_06_27_21132_creeate_tasks_table.php
```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateTasksTable extends Migration
{
   public function up()
   {
       Schema::create('tasks', function (Blueprint $table) {
           $table->id();
           $table->string('title');
           $table->text('description');
           $table->boolean('status')->default(false);
           $table->timestamps();
       });
   }

   public function down()
   {
       Schema::dropIfExists('tasks');
   }
```
### Execução das migrações e configuração do banco de dados
Antes de prosseguir, devemos configurar o arquivo ```.env``` com as informações do banco de dados que iremos utilizar. Em seguida, executamos o comando ```php artisan migrate``` para criar as tabelas no banco de dados. Isso criará a tabela "tasks" com as colunas apropriadas.

### Implementação da lógica nos controladores
Agora, podemos implementar a lógica necessária em cada método dos controladores para realizar as operações desejadas. Por exemplo, no método ```store``` do controlador de tarefas, podemos receber os dados da nova tarefa, criar uma instância do modelo "Task" e salvá-la no banco de dados.

### Teste da API no Postman
Com a API implementada, podemos utilizar o Postman para testar os endpoints e verificar se tudo está funcionando conforme o esperado. Podemos enviar requisições para criar, recuperar, atualizar e excluir tarefas, verificando as respostas e os resultados no banco de dados.

### Teste da API Laravel no Postman:
[![](https://markdown-videos.vercel.app/youtube/Mk82GSmrk4s)](https://www.youtube.com/watch?v=Mk82GSmrk4s)

