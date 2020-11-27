# curd-project
Crud.php

<?php  
namespace App;  
use Illuminate\Database\Eloquent\Model;  
class Crud extends Model  
{  
    //  
protected $table='curdtable';  
protected $fillable=['id','name','rate','category','qty'];  
}  



CrudControl.blude.php


<?php   
namespace App\Http\Controllers;  
use Illuminate\Http\Request;  
class CrudsController extends Controller  
{  
    /** 
     * Display a listing of the resource. 
     * 
     * @return \Illuminate\Http\Response 
     */  
    public function index()  
    {  
        $cruds = Crud::all();  
  
        return view('index', compact('cruds')); 
    }  
  
    /** 
     * Show the form for creating a new resource. 
     * 
     * @return \Illuminate\Http\Response 
     */  
    public function create()  
    {  
        //  
    }  
  
    /** 
     * Store a newly created resource in storage. 
     * 
     * @param  \Illuminate\Http\Request  $request 
     * @return \Illuminate\Http\Response 
     */  
    public function store(Request $request)  
    {  
        $request->validate([  
            'name'=>'required',  
            'rate'=>'required',  
            'category'=>'required',  
            'qty'=>'required'  
        ]);  
  
        $crud = new Crud;  
        $crud->name =  $request->get('name');  
        $crud->rate = $request->get('rate');  
        $crud->category = $request->get('category');  
        $crud->qty = $request->get('qty');  
        $crud->save();  
    }  
  
    /** 
     * Display the specified resource. 
     * 
     * @param  int  $id 
     * @return \Illuminate\Http\Response 
     */  
    public function show($id)  
    {  
        //  
    }  
  
    /** 
     * Show the form for editing the specified resource. 
     * 
     * @param  int  $id 
     * @return \Illuminate\Http\Response 
     */  
    public function edit($id)  
    {  
       $crud= Crud::find($id);  
     return view('edit', compact('crud'));
    }  
  
    /** 
     * Update the specified resource in storage. 
     * 
     * @param  \Illuminate\Http\Request  $request 
     * @param  int  $id 
     * @return \Illuminate\Http\Response 
     */  
    public function update(Request $request, $id)  
    {  
        $request->validate([  
            'name'=>'required',  
            'rate'=>'required',  
            'category'=>'required',  
            'qty'=>'required'  
        ]);  
  
        $crud = Crud::find();  
        $crud->name =  $request->get('name');  
        $crud->rate = $request->get('rate');  
        $crud->category = $request->get('category');  
        $crud->qty = $request->get('qty');  
        
        $crud->save();    
    }  
  
    /** 
     * Remove the specified resource from storage. 
     * 
     * @param  int  $id 
     * @return \Illuminate\Http\Response 
     */  
    public function destroy($id)  
    {  
        $crud=Crud::find($id);  
        $crud->delete();  
    }  
}  


index.blade.php

@extends('layout.master')  
@section('content')  
<table border="1px">  
<thead>  
<tr>  
<td>  
ID </td>  
<td>  
Name </td>  
<td>  
Rate</td>  
<td>  
Category</td>  
<td>  
Qty</td>  
</tr>  
</thead>  
<tbody>  
@foreach($cruds as $crud)  
        <tr border="none">  
            <td>{{$crud->id}}</td>  
            <td>{{$crud->name}}</td>  
            <td>{{$crud->rate}}</td>  
            <td>{{$crud->category}}</td>  
            <td>{{$crud->qty}}</td>  
<td >  
<form action="{{ route('users.destroy', $crud->id)}}" method="post">  
                  @csrf  
                  @method('DELETE')  
                  <button class="btn btn-danger" type="submit">Delete</button>  
                </form>  
</td>  
<td >  
<form action="{{ route('users.edit', $crud->id)}}" method="GET">  
                  @csrf  
                   
                  <button class="btn btn-danger" type="submit">Edit</button>  
                </form>  
</td>  
  
         </tr>  
@endforeach  
</tbody>  
</table>  
@endsection



create.blade.php

@extends('layout.master')  
@section('content')  
<form method="post" action="{{ route('users.store') }}">  
   @csrf     
          <div class="form-group">      
              <label for="id">Id:</label><br/><br/>  
              <input type="text" class="form-control" name="id"/><br/><br/>  
          </div>  
<div class="form-group">      
<label for="name">Name:</label><br/><br/>  
              <input type="text" class="form-control" name="name"/><br/><br/>  
          </div>  
<div class="form-group">      
              <label for="rate">Rate:</label><br/><br/>  
              <input type="text" class="form-control" name="rate"/><br/><br/>  
          </div>  
<div class="form-group">      
              <label for="category">Category:</label><br/><br/>  
              <input type="text" class="form-control" name="category"/><br/><br/>  
          </div> 
<div class="form-group">      
              <label for="qty">Qty:</label><br/><br/>  
              <input type="text" class="form-control" name="qty"/><br/><br/>  
          </div>  
<br/>  
<button type="submit" class="btn-btn" >Insert Category</button>  
</form>  
@endsection  




edit.blade.php

@extends('layout.master')  
@section('content')  
<form method="Post" action="{{route('users.update',$crud->id)}}">  
@method('PATCH')     
 @csrf     
          <div class="form-group">      
              <label for="name">Name:</label><br/><br/>  
              <input type="text" class="form-control" name="name" value={{$crud->name}}><br/><br/>  
          </div>  
  
<div class="form-group">      
              <label for="rate">Rate:</label><br/><br/>  
              <input type="text" class="form-control" name="rate" value={{$crud->rate}}><br/><br/>  
          </div>  
<div class="form-group">      
              <label for="category">Category:</label><br/><br/>  
              <input type="text" class="form-control" name="category" value={{$crud->category}}><br/><br/>  
          </div>  
<div class="form-group">      
              <label for="qty">Qty:</label><br/><br/>  
              <input type="text" class="form-control" name="qty" value={{$crud->qty}}><br/><br/>  
          </div>  
<br/>  
  
<button type="submit" class="btn-btn" >Update</button>  
</form>  
  
  
@endsection 





web.php

<?php

use Illuminate\Support\Facades\Route;

$ADMIN_PREFIX = "admin";

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/insert', function () {  
    return view('create');  
});  

Route::get('/', function () {
    return view('welcome');
});




.env


APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:S2rBU/nnNSE7HznAAS7j8CyOV0GIyLq0Y4SmvvZOHyQ=
APP_DEBUG=true
APP_URL=http://localhost/spa/

LOG_CHANNEL=stack
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=curd
DB_USERNAME=root
DB_PASSWORD=

BROADCAST_DRIVER=log
CACHE_DRIVER=file
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=null
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_APP_CLUSTER=mt1

MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"




