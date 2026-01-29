demo of out-of-the-box-valid purl generation inside nixpkgs: https://github.com/NixOS/nixpkgs/pull/421125

build nixtract with upstreamed patches (no new version yet):

```
(pkgs.nixtract.overrideAttrs({ patches = [ ./nixtract-purl.patch ./nixtract-purl2.patch ]; }))
```

```
nixtract --target-attribute-path python312Packages --target-flake-ref github:NixOS/nixpkgs/54271156702fc3a3f5d156df567a2a4a274bac6b > output.json
cat output.json | jq -r '.nixpkgs_metadata.purl + " " + .nixpkgs_metadata.pname' | sort | uniq > purls.txt
```

python312Packages contains around 10238 first-level derivations: https://search.nixos.org/packages?channel=25.11&query=python312Packages
```
bash-5.3# zcat output.json.gz | jq -r '.nixpkgs_metadata.purl + " " + .nixpkgs_metadata.pname' | sort | uniq | grep "pkg:" | wc -l
10574
bash-5.3# zcat output.json.gz | jq -r '.nixpkgs_metadata.purl + " " + .nixpkgs_metadata.pname' | sort | uniq | grep -v "pkg:" | wc -l
1911
```

out of them around 8k are github and around 17% of them have a homepage different to the code source
```
bash-5.3# zcat output.json.gz | jq -r '.nixpkgs_metadata.purl + " " + .nixpkgs_metadata.pname + " " + .nixpkgs_metadata.homepage' | sort | uniq | grep ":github" | wc -l
6711
bash-5.3# zcat output.json.gz | jq -r '.nixpkgs_metadata.purl + " " + .nixpkgs_metadata.pname + " " + .nixpkgs_metadata.homepage' | sort | uniq | grep ":github" | grep -v "https://github.com" | wc -l
1111
```

