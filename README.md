<h1 align="center">OVPN Config Manipulator</h1>
<p align="center">
<a href="https://github.com/labbots/OVPN-Config-Manipulator/releases"><img src="https://img.shields.io/github/release/labbots/OVPN-Config-Manipulator.svg?style=for-the-badge" alt="Latest Release"></a>
<a href="https://github.com/labbots/OVPN-Config-Manipulator/blob/master/LICENSE"><img src="https://img.shields.io/github/license/labbots/OVPN-Config-Manipulator.svg?style=for-the-badge" alt="License"></a>
<a href="https://www.codacy.com/manual/labbots/OVPN-Config-Manipulator?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=labbots/OVPN-Config-Manipulator&amp;utm_campaign=Badge_Grade"><img alt="Codacy grade" src="https://img.shields.io/codacy/grade/d1cafdb781464fab858155b31116f2e7?style=for-the-badge"></a>
</p>

Open VPN config file (.ovpn) contains several certificates and key files which are required for the setup. This script allows you to merge those certificates and keys into single config file. The script can also be used to split a single self-contained ovpn config file to individual config and cert files.

## Dependencies

This script does not have very many dependencies. Most of the dependencies are available by default in most linux platforms. This script requires the following packages.

- sed (Stream editor)
- grep
- awk
- getopt
- xargs

## Usage
The script can be used to merge or split OVPN file.

Basic Usage:

```shell
./OVPN-config-manipulator.sh [options..] <ovpn-config-file> 
```

To split a OVPN file into main config file and relevant cert and key files, the script can be used as follows

```shell
./OVPN-config-manipulator.sh -p -s home/openvpn/vpn.ovpn -d home/openvpn/split/
```

In the above example `-p` option specifies split operation. The source file is read from location specified in `-s` option. The final split files are stored in the directory specified in `-d` option.

To merge OVPN config file and relevant certificates, the script can be used as follows

```shell
./OVPN-config-manipulator.sh -m=auto -s home/openvpn/vpn.ovpn -d home/openvpn/merged/ \
    --ca home/openvpn/vpn-ca.crt \
    --cert home/openvpn/vpn-client.crt \
    --key home/openvpn/vpn-client.key
```

In the above example `-m` option indicates merge operation. The source file is loaded from file specified in `-s` option and output single merged config file is saved to destination folder specified in `-d` option. The `auto` option set to `-m` flag will make the script to auto detect ca file, certificate, key, tls-auth certificate and dh-params files from path specified within the OVPN file. If those values exists within the OVPN config file then the script automatically tries to read content from the location and link it to the file. The values can be overriden by options provided to the script. So in the above example Certificate authority file will be loaded from location provided by `--ca` option even if the path is different in the OVPN config file. 

List of options available in the script are

```
    -p | --split - flag to split ovpn file.
    -s<filepath> | --source <filepath> - Option to pass the source ovpn file.
    -d<folderpath> | --destination <folderpath> - Destination location where the newly created config to be stored. 
    -m | --merge [optional parameter : auto] - flag to merge file. takes a optional parameter [auto].
    if auto is set, then the script tries to identify certificates and keys from path specified in ovpn file. This can be overridden by other options.
    --ca <filepath> - option to specify the location of the CA file. 
    --cert <filepath>  - option to specify the location of the certificate file.
    --key <filepath>  - option to specify the location of the key file. 
    --tls-auth <filepath>  - option to specify the location of the tls-auth file.
    --dh-params <filepath> - opton to specify the location of the dh params file.
    -h | --help - Display usage instructions.
```

## License

MIT