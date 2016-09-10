# kcptun_mip32le
add precompiled kcptun client for mip32le

#build steps
* pull the go-mips32le env 
 
 `docker run -t -i shutup/docker-go-mips32le /bin/sh`
* add build.sh & run
<pre>
go get -v golang.org/x/crypto/pbkdf2
go get -v github.com/xtaci/kcp-go
go get -v github.com/urfave/cli
#older version using yamux
#go get -v github.com/hashicorp/yamux
#newer version using smux
go get -v github.com/xtaci/smux
go get -v github.com/golang/snappy
go get -v golang.org/x/net/ipv4
go get -v github.com/xtaci/kcptun/client
VERSION=`date -u +%Y%m%d`
LDFLAGS="-X main.VERSION=$VERSION -s -w"
go build -ldflags "$LDFLAGS" -o client_linux_mips github.com/xtaci/kcptun/client
</pre>
* add patch

modify gen.go (/go/src/golang.org/x/net/ipv4/gen.go) add mips32le support

before:
<pre>
case runtime.GOOS == "linux" && (runtime.GOARCH == "arm64" || runtime.GOARCH == "mips64" || runtime.GOARCH == "mips64le" || runtime.GOARCH == "ppc64" || runtime.GOARCH == "ppc64le" || runtime.GOARCH == "s390x"):
</pre>
after:
<pre>
case runtime.GOOS == "linux" && (runtime.GOARCH == "arm64" || runtime.GOARCH == "mips64" || runtime.GOARCH == "mips64le" || runtime.GOARCH == "ppc64" || runtime.GOARCH == "ppc64le" || runtime.GOARCH == "s390x" || || runtime.GOARCH == "mips32le"):
</pre>
copy `zsys_linux_mips64le.go` to `zsys_linux_mips32le.go`

before:
<pre>
// +build linux,mips64le
</pre>
after:
<pre>
// +build linux,mips32le
</pre>

* after patch
  
  rerun `go get -v golang.org/x/net/ipv4`
* if no fails,then run the `buils.sh`

#attention
the compression is not support well,so just turn it off
