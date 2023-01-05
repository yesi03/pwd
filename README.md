<?php
// Koneksi ke database
$host = 'localhost';
$user = 'username';
$password = 'password';
$dbname = 'nama_database';

// Buat koneksi
$conn = mysqli_connect($host, $user, $password, $dbname);
// Cek koneksi
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}

// Buat tabel jika belum ada
$sql = "CREATE TABLE IF NOT EXISTS coffee (
	id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY, 
	name VARCHAR(30) NOT NULL,
	price DECIMAL(5,2) NOT NULL
)";
mysqli_query($conn, $sql);

// Fungsi untuk membuat data
function createData($name, $price) {
	global $conn;
	$sql = "INSERT INTO coffee (name, price) VALUES ('$name', '$price')";
	if (mysqli_query($conn, $sql)) {
	    echo "New record created successfully";
	} else {
	    echo "Error: " . $sql . "<br>" . mysqli_error($conn);
	}
}

// Fungsi untuk membaca data
function readData() {
	global $conn;
	$query = "SELECT * FROM coffee";
	$result = mysqli_query($conn, $query);
	if (mysqli_num_rows($result) > 0) {
	    // output data of each row
	    while($row = mysqli_fetch_assoc($result)) {
	        echo "id: " . $row["id"]. " - Name: " . $row["name"]. " " . $row["price"]. "<br>";
	    }
	} else {
	    echo "0 results";
	}
}

// Fungsi untuk mengubah data
function updateData($id, $name, $price) {
	global $conn;
	$sql = "UPDATE coffee SET name='$name', price='$price' WHERE id=$id";
	if (mysqli_query($conn, $sql)) {
	    echo "Record updated successfully";
	} else {
	    echo "Error: " . $sql . "<br>" . mysqli_error($conn);
	}
}

// Fungsi untuk menghapus data
function deleteData($id) {
	global $conn;
	$sql = "DELETE FROM coffee WHERE id=$id";
	if (mysqli_query($conn, $sql)) {
	    echo "Record deleted successfully";
} else {
	    echo "Error: " . $sql . "<br>" . mysqli_error($conn);
	}
}

// Contoh penggunaan fungsi
createData("Espresso", 2.50);
readData();
updateData(1, "Latte", 3.50);
readData();
deleteData(1);
readData();

// Tutup koneksi
mysqli_close($conn);
?>
