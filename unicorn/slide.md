!SLIDE center

![](candy_mountain.jpg)



!SLIDE bullets incremental

# killer-ficzery

* zerowy downtime przy restarcie aplikacji
* aplikacja ładowana z dysku **raz**
* master zarządza workerami
* zarządzanie masterem -- sygnały

!SLIDE bullets incremental

# hot restart

* nowy master startuje
* pierwszy nowy worker ubija starego mastera
* czary!



!SLIDE bullets incremental left-img

# jednokrotne ładowanie

![](charlie_poster.jpeg)

* master ładuje kod aplikacji,
* workery otrzymują kod przez kopię pamięci
* fork() FTW!



!SLIDE

# zarządzanie Unicornem

    @@@ ruby
    namespace :deploy do
      task :start, :roles => :app do
        run "unicorn_rails -c #{current_path}/config/unicorn.rb -E production -D"
      end
      
      task :restart, :roles => :app do
        # USR2 causes the master to re-create itself and spawn a new worker pool
        run "kill -USR2 `cat /tmp/unicorn.pid`"
      end
      
      task :stop, :roles => :app do
        # QUIT gracefully shuts down workers
        run "kill -QUIT `cat /tmp/unicorn.pid`"
      end
      
      task :more_workers, :roles => :app do
        run "kill -TTIN `cat /tmp/unicorn.pid`"
      end
    end
    



!SLIDE

# konfigurowanie Unicorna

## plik ruby o bardzo prostej składni

    @@@ ruby
    rails_env = ENV['RAILS_ENV'] || 'production'
    
    # 16 workers and 1 master
    worker_processes (rails_env == 'production' ? 16 : 4)
    
    # Listen on a Unix data socket
    listen '/tmp/unicorn.sock', :backlog => 2048
    #listen 8080, :tcp_nopush => true
    
    pid '/tmp/unicorn.pid'



!SLIDE bullets

# uwaga na

* rack 1.1 (lepiej zostać przy 1.0.1)
* ustawienia do restartu po deploy



!SLIDE center

# I Ty możesz mieć Unicorna!

## warto spróbować



!SLIDE

# Spis lektur

    http://unicorn.bogomips.org

    http://github.com/blog/517-unicorn

    http://tomayko.com/writings/unicorn-is-unix

    http://tomash.wrug.eu/2010/01/30/the-awesomeness-of-unicorn.html
    
    
!SLIDE

# Dziękuję

### http://tomash.wrug.eu
