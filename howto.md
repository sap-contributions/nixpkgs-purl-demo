
(pkgs.nixtract.overrideAttrs({ patches = [ ./nixtract-purl.patch ./nixtract-purl2.patch ]; }))

nixtract --target-attribute-path python312Packages --target-flake-ref github:NixOS/nixpkgs/54271156702fc3a3f5d156df567a2a4a274bac6b > output.json
