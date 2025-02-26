provider "google" {
    project = "siva-sai-447213"
    region =  "us-central1"
    credentials = file("key.json")
}
resource "google_compute_network" "network-1" {
    name = "network-1"
    auto_create_subnetworks = false
}
resource "google_compute_subnetwork" "subnet-a" {
    name = "subnet-a"
    region = "us-central1"
    ip_cidr_range = "10.0.0.0/24"
    network = google_compute_network.network-1.id
}
 resource "google_compute_disk" "mydisk" {
        name = "disk-1"
        size = 15
        type = "pd-standard"
        zone = "us-central1-a"
}
resource "google_compute_instance" "vm-1" {
    name =  "vm-1"
    zone = "us-central1-a"
    machine_type = "e2-medium"

    boot_disk {
        initialize_params {
            image = "debian-cloud/debian-11"
            size = 10
            type = "pd-standard"
        }
    }
    attached_disk {
        source = google_compute_disk.mydisk.id
        device_name = "disk-1"
    }
    network_interface {
        subnetwork = google_compute_subnetwork.subnet-a.id
        access_config{}
    }
}
resource "google_compute_firewall" "allow-all" {
    name = "allow-all"
    network = google_compute_network.network-1.id
    allow{
        protocol = "icmp"
    }
    allow{
        protocol = "tcp"
        ports = ["22"]
    }
    source_ranges =["0.0.0.0/0"]
    
}
