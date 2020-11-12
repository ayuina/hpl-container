# HPL : High Perfocmance LINPACK を Docker コンテナで実行する

[こちら](https://sites.google.com/site/rangsiman1993/comp-env/test-and-benchmarks/build-hpl-with-blas-and-mpi)
を参考にして、Ubuntu コンテナで HPL を実行にすることができました。

- OpenBLAS をビルドしてインストールする
- OpenMPI をビルドしてインストール
- HPL をビルド

コンテナサイズの最適化などまだやるべきことは多いのですが、さしあたり暫定版として公開します。

## How to build

```bash
cd ./linux64
docker build -t hpl .
```

## How to run

```bash
docker run -it --rm hpl /bin/bash -c 'mpirun -np 4 xhpl'
```

`mpirun` 実行時に `root` だと権限が強すぎるといって怒られるので、実行ユーザーを変更するか `--allow-run-as-root` オプション付きで実行してみてください。
下記のような形で出力されると思います。

```
================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WR00C2R4       16128   384     2     1             204.79             1.3659e+01
HPL_pdgesv() start time Thu Nov 12 04:25:49 2020

HPL_pdgesv() end time   Thu Nov 12 04:29:14 2020

[bee7252ab0cc:00010] Read -1, expected 129024, errno = 1
[bee7252ab0cc:00010] Read -1, expected 129024, errno = 1
--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   2.62156040e-03 ...... PASSED
================================================================================

Finished      1 tests with the following results:
              1 tests completed and passed residual checks,
              0 tests completed and failed residual checks,
              0 tests skipped because of illegal input values.
--------------------------------------------------------------------------------

End of Tests.
================================================================================
```

## How to tuning

現状の Dockerfile ではベンチマークの実行条件である `HPL.dat` はコンテナイメージのビルド時にコピーする作りになっていますので、このままだと毎回同じ条件でしか実行できません。
条件を変えて実行されたい場合には、先ほどビルドしたイメージをベースにして HPL.data を `WORKDIR` にコピーする Dockerfile を用意してイメージをビルドすると手っ取り早いかと思います。

