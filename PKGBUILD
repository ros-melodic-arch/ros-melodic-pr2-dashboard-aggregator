# Maintainer: Timon Engelke <aur@timonengelke.de>
pkgdesc="ROS - A simple script that aggregates all of the topics that a \"pr2_dashboard\" app might be interested in."
url='http://wiki.ros.org/pr2_dashboard_aggregator'

pkgname='ros-melodic-pr2-dashboard-aggregator'
pkgver='1.12.3'
_pkgver_patch=0
arch=('any')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-melodic-catkin)
makedepends=('cmake' 'ros-build-tools'
  ${ros_makedepends[@]})

ros_depends=(ros-melodic-std-msgs
  ros-melodic-pr2-msgs
  ros-melodic-rospy)
depends=(${ros_depends[@]})

_dir="pr2_common-release-release-melodic-pr2_dashboard_aggregator-${pkgver}-${_pkgver_patch}"
source=("${pkgname}-${pkgver}-${_pkgver_patch}.tar.gz"::"https://github.com/pr2-gbp/pr2_common-release/archive/release/melodic/pr2_dashboard_aggregator/${pkgver}-${_pkgver_patch}.tar.gz")
sha256sums=('50c10a982c9d7fe6b1d6f64f73bcdaa207b5a8f76ddd867ff146114e1ba020c4')

build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python3.7 \
        -DPYTHON_LIBRARY=/usr/lib/libpython3.7.so \
        -DPYTHON_BASENAME=-python3.7 \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
