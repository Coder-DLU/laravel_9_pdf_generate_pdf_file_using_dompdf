# laravel_9_pdf_generate_pdf_file_using_dompdf
## 1. Install Laravel 9
```Dockerfile
composer create-project laravel/laravel example-app
```
## 2. Install DomPDF Package
```Dockerfile
composer require barryvdh/laravel-dompdf
```
## 3. Create Controller
- chúng ta sẽ tạo PDFController với createPDF (), nơi chúng tôi viết mã tạo pdf. vì vậy hãy tạo bộ điều khiển bằng lệnh dưới đây.
```Dockerfile
php artisan make:controller PDFController
```
- trong PDFController, chúng ta cũng lấy dữ liệu bảng của người dùng và hiển thị chúng thành tệp pdf. vì vậy bạn có thể thêm một số dữ liệu giả trên bảng người dùng bằng cách sử dụng lệnh tinker sau:
```Dockerfile
php artisan tinker
User::factory()->count(10)->create()
``` 
-cập nhật lại code trong file controller file Vào app/Http/Controllers/PDFController.php  
```Dockerfile
<?php
  
namespace App\Http\Controllers;
  
use Illuminate\Http\Request;
use App\Models\User;
use PDF;
  
class PDFController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function generatePDF()
    {
        $users = User::get();
  
        $data = [
            'title' => 'Welcome to ItSolutionStuff.com',
            'date' => date('m/d/Y'),
            'users' => $users
        ]; 
            
        $pdf = PDF::loadView('myPDF', $data);
     
        return $pdf->download('itsolutionstuff.pdf');
    }
}
``` 
## 4. Add Route
- Vào routes/web.php
```Dockerfile
<?php
  
use Illuminate\Support\Facades\Route;
  
use App\Http\Controllers\PDFController;
  
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
  
Route::get('generate-pdf', [PDFController::class, 'generatePDF']);
```
## 5. Create View File
- Vào resources/views/myPDF.blade.php
```Dockerfile
<!DOCTYPE html>
<html>
<head>
    <title>Laravel 9 Generate PDF Example - ItSolutionStuff.com</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>
    <h1>{{ $title }}</h1>
    <p>{{ $date }}</p>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua.</p>
  
    <table class="table table-bordered">
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
        </tr>
        @foreach($users as $user)
        <tr>
            <td>{{ $user->id }}</td>
            <td>{{ $user->name }}</td>
            <td>{{ $user->email }}</td>
        </tr>
        @endforeach
    </table>
  
</body>
</html>
``` 
## 6. Run Laravel App:
```Dockerfile
php artisan serve
``` 
- Vào http://localhost:8000/generate-pdf
bạn sẽ tải xuống tệp như dưới đây:
laravel-9-pdf-file.pdf trong kho chứa của tôi
Bây giờ chúng tôi đã sẵn sàng để chạy ví dụ này và kiểm tra nó ...
