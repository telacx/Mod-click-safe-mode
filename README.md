import os, json, zipfile

root = "/mnt/data/ClickSafeGD-Android-Project"
mod_path = os.path.join(root, "mod.json")
with open(mod_path, encoding="utf-8") as f:
    mod = json.load(f)
# Geode core versions and Android launcher versions are separate; v4.10.2 is documented as matching GD 2.2074.
mod["geode"] = "v4.10.2"
with open(mod_path, "w", encoding="utf-8") as f:
    json.dump(mod, f, indent=2)
    f.write("\n")

readme_path = os.path.join(root, "README.md")
with open(readme_path, encoding="utf-8") as f:
    readme = f.read()
readme = readme.replace(
    "Targets Android Geometry Dash `2.2074` and Geode `v1.8.0` in `mod.json`.",
    "Targets Android Geometry Dash `2.2074` and Geode core `v4.10.2` in `mod.json`."
)
readme = readme.replace(
    "If Geode `v1.8.0` does not ship binaries for Android GD `2.2074`, the build will stop. In that case, install the Geode SDK/binaries that match the Geode version installed in the game and update the `geode` value in `mod.json` to that exact SDK version. Do not guess this field.",
    "The Android launcher version (for example, `1.8.0`) is not necessarily the Geode core/SDK version in `mod.json`. The project currently targets Geode core `v4.10.2`, reported as compatible with GD `2.2074`. The build must still be run against SDK binaries that match your actual game/Geode installation; if your installed core differs, update `geode` to the exact SDK version and verify the target GD version before compiling. Do not guess this field."
)
with open(readme_path, "w", encoding="utf-8") as f:
    f.write(readme)

zip_path = "/mnt/data/ClickSafeGD-Android-Project.zip"
files = ["src/ClickSafeGD.cpp", "mod.json", "CMakeLists.txt", "README.md", "about.md", "changelog.md"]
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    for rel in files:
        z.write(os.path.join(root, rel), arcname="ClickSafeGD-Android-Project/" + rel)

print("Updated project:", zip_path)
print("mod.json Geode target:", mod["geode"])
geode sdk install-binaries -p android64
geode build -p android64geode sdk install
geode sdk install-binaries -p android64geode build -p android64
