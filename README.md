# rust-bitcoin-gui
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
cargo new rust-bitcoin-gui
cd rust-bitcoin-gui
cargo add bitcoin
cargo add egui eframe
[package]
name = "rust-bitcoin-gui"
version = "0.1.0"
edition = "2021"

[dependencies]
bitcoin = "0.29.2"
egui = "0.20.1"
eframe = "0.20.0" # eframe with egui


use bitcoin::{Address, Network};
use eframe::egui;

fn main() {
    let native_options = eframe::NativeOptions::default();
    eframe::run_native("Bitcoin Node GUI", native_options, Box::new(|cc| Box::new(NodeGui::new(cc))));
}

struct NodeGui {
    address: String,
}

impl NodeGui {
    fn new(cc: &eframe::CreationContext<'_>) -> Self {
        Self {
            address: String::new(),
        }
    }
}

impl eframe::App for NodeGui {
    fn update(&mut self, ctx: &egui::Context, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ctx, |ui| {
            ui.heading("Bitcoin Node GUI");
            ui.label("Enter your Bitcoin address:");
            ui.text_edit_singleline(&mut self.address);
            
            if ui.button("Validate Address").clicked() {
                match Address::from_str(&self.address, Network::Bitcoin) {
                    Ok(address) => {
                        ui.label(format!("Valid Bitcoin address: {}", address));
                    }
                    Err(e) => {
                        ui.label(format!("Invalid Bitcoin address: {}", e));
                    }
                }
            }
        });
    }}
cargo run
git init
git add .
git commit -m "Initial commit with basic GUI"
git remote add origin git@github.com:TwojaNazwaUzytkownika/rust-bitcoin-gui.git
git push -u origin main
